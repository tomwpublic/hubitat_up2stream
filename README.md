# hubitat_up2stream
This driver provides Hubitat-based AudioVolume, MusicPlayer, and SpeechSynthesis capabilities for use with up2stream devices, including the Up2Stream players from Arylic and other players based on them (e.g. Dayton Audio WBA31 and AudioCast).

Note that I have no affiliation with Hubitat, Arylic, Dayton Audio, AudioCast, or any other hardware or software providers that may be referenced in this software package.

# installation instructions
* Install 'up2stream player' and 'up2stream group' as drivers in the Advanced > Drivers Code section of the Hubitat web interface
  * If you plan to use multiple up2stream devices, skip ahead to the next section.
  * If you have a single up2stream device, go to the Devices section of the Hubitat web interface, and use Add Virtual Device to install an 'up2stream player' device.
    * After the device is created, enter the player IP address and Save Preferences and the driver will attempt to establish communication.
* Install 'up2stream manager' and 'up2stream manager instance' as apps in the Advanced > Apps Code section of the hubitat web interface
  * Install the 'up2stream manager' in the Apps section of the Hubitat web interface by using Add User App.  Click through any prompts to install the parent app.
  * Visit the 'up2stream manager' app on the Apps page to create an 'up2stream manager instance.'  Use the web interface to discover devices on your network and to select the group master device and any slaves that you want to have associated with the group.
    * Be sure to give the group a recognizable name.  When you complete the installation, Hubitat virtual devices will be created to access and control the group.

# player usage
* This section is mostly self-explanatory.
* In order to learn the URI format used by various audio sources, I recommend using the manufacturer's mobile device app to play a track from your desired source and then note the URI in the device attributes in Hubitat.
 * Many of the internet radio sources (e.g. TuneIn) seem to have URIs that do not vary much over time for a given source.
 * I have not found a reliable way to generate Amazon or Spotify URIs.  If anyone can deduce the format, please share that info and I'll update these instructions.
 
 # group usage
 * If you are planning to use the grouping features, I strongly recommend installing and creating devices via the 'up2stream manager' app.
 * The virtual device structure is as follows: 
   * *\<name\>-group*: this is the parent virtual device for the group and provides all of the music-related features as well as grouping control.  I recommend using this device for all operations.
   * *\<name\>-group-\<masterIP\>*: this is an 'up2stream player' virtual device that addresses the group master.  It is possible to use this device when using the master individually, but it is not recommended when the master is grouped with any slaves.
   * *\<name\>-group-slave*: this is an 'up2stream player' virtual device that is used internally by the group parent device.  You should never use or change any settings on this device.
 * Binding or Unbinding the whole group:
   * *push(1)* (pushing button number 1) will attempt to bind all slaves selected in the 'up2stream manager instance' to the group master.
   * *push(0)* and *hold(1)* will both unbind all slaves currently bound to the group master.
* Binding or Unbinding individual slaves:
  * *push(slaveIP)* will attempt to bind a slave at that IP address to the group master.
  * *push(slaveIP)* for a slave that is already bound will result in that slave being unbound from the group master.
  * Once a slave has been bound at least once by IP, it can also be referenced by its name in the above operations (e.g. *push(Amp1)*)
  * The 'up2stream group' virtual device has a *currentSlaves* attribute which shows the names of any slaves that are currently bound, or (none).
  
# hubitat dashboard
* Music Player, Volume, Button, and Attribute templates all function as expected.
 * Create multiple dashboard entries and point them all to the 'up2stream group' virtual device.
   * For Music Player and Volume, be sure to reference the 'up2stream group' parent device and not either of the 'up2stream player' child devices.
   * For buttons, use numbers (e.g. push(1)), IP addresses (e.g. push(10.0.0.1)), or slave names (e.g. push(Amp1))
   * Use an Attribute template to display the currentSlaves list.
