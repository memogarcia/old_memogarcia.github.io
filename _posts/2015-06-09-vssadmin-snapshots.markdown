---
layout: post
title:  "Snapshots on windows using vssadmin"
date:   2015-06-08 15:23:00
categories: python windows snapshots powershell
---

### Create a snapshot in windows (from xp to 10)


Create a powershell script (vss.ps1)


{% highlight powershell %}

param([String]$volume="")

$shadow = get-wmiobject win32_shadowcopy

# get static method
$class=[WMICLASS]"root\cimv2:win32_shadowcopy"

# create a new shadow copy
$s1 = $class.create($volume, "ClientAccessible")

# get shadow ID
$s2 = gwmi Win32_ShadowCopy | ? { $_.ID -eq $s1.ShadowID }

$d  = $s2.DeviceObject + "\"

# create a symlink for the shadow path
cmd /c mklink /d $volume\shadowcopy "$d"

echo "shadow id:" $s2

{% endhighlight %}


helper for suprocess

{% highlight python %}

def create_subprocess(cmd):
    """
    Create a new subprocess in the OS
    :param cmd: command to execute in the subprocess
    :return: the output and errors of the subprocess
    """
    process = subprocess.Popen(cmd,
                               stdin=subprocess.PIPE,
                               stdout=subprocess.PIPE,
                               stderr=subprocess.PIPE)
    return process.communicate()

{% endhighlight %}


create a snapshot with python

{% highlight python %}

def vss_create_shadow_copy(volume):
    """
    Create a new shadow copy for the specified volume

    Windows registry path for vss:
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VSS\Settings

    MaxShadowCopies
    Windows is limited in how many shadow copies can create per volume.
    The default amount of shadow copies is 64, the minimum is 1 and the maxi-
    mum is 512, if you want to change the default value you need to add/edit
    the key MaxShadowCopies and set the amount of shadow copies per volume.

    MinDiffAreaFileSize
    The minimum size of the shadow copy storage area is a per-computer setting
    that can be specified by using the MinDiffAreaFileSize registry value.

    If the MinDiffAreaFileSize registry value is not set, the minimum size of
    the shadow copy storage area is 32 MB for volumes that are smaller than
    500 MB and 320 MB for volumes that are larger than 500 MB.

    If you have not set a maximum size, there is no limit to the amount
    of space that can be used.

    If the MinDiffAreaFileSize registry value does not exist, the backup
    application can create it under the following registry key:

    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\VolSnap


    Freezer create a shadow copy for each time the client runs it's been
    removed after the backup is complete.

    :param volume: The letter of the windows volume e.g. c:\\
    :return: shadow_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    :return: shadow_path: shadow copy path
    """
    shadow_path = None
    shadow_id = None
    with DisableFileSystemRedirection():
        script = 'vss.ps1'  # powershell script
        (out, err) = create_subprocess(['powershell.exe',
                                        '-executionpolicy', 'unrestricted',
                                        '-command', script,
                                        '-volume', volume])
        if err != '':
            raise Exception('[*] Error creating a new shadow copy on {0}'
                            ', error {1}' .format(volume, err))

        for line in out.split('\n'):
            if 'symbolic' in line:
                shadow_path = line.split('>>')[1].strip()
            if '__RELPATH' in line:
                shadow_id = line.split('=')[1].strip().lower() + '}'
                shadow_id = shadow_id[1:]

        logging.info('[*] Created shadow copy {0}'.
                     format(shadow_id))

        return shadow_path, shadow_id

{% endhighlight %}


Delete a shadow copy

{% highlight python %}

def vss_delete_shadow_copy(shadow_id, volume):
    """
    Delete a shadow copy from the volume with the given shadow_id
    :param shadow_id: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
    :return: bool
    """

    with DisableFileSystemRedirection():
        cmd = ['vssadmin', 'delete', 'shadows',
               '/shadow={0}'.format(shadow_id), '/quiet']
        (out, err) = create_subprocess(cmd)
        if err != '':
            raise Exception('[*] Error deleting shadow copy with id {0}'
                            ', error {1}' .format(shadow_id, err))

        try:
            os.rmdir(os.path.join(volume, 'shadowcopy'))
        except Exception:
            logging.error('Failed to delete shadow copy symlink {0}'.
                          format(os.path.join(volume, 'shadowcopy')))

        logging.info('[*] Deleting shadow copy {0}'.
                     format(shadow_id))

        return True

{% endhighlight %}