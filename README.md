**!!! DISCLAIMER !!!**: The current code currently wraps the **new launcher**! Management of the old launcher has drastically changed. Version [1.0](https://github.com/ReflxctionDev/MinecraftLauncherLibrary/releases/tag/1.0) has full support
for the old launcher, and 1.0 source code can be found in the [old-launcher](https://github.com/ReflxctionDev/MinecraftLauncherLibrary/tree/old-launcher) branch of this repository tree.

# Minecraft Launcher Library
This library gives full access to any exposed data in Minecraft's launcher. This includes:

* **Profiles Management**: You can create launcher profiles and remove them, with flexible methods and full control over the profile. (Examples below)
* **Authentication Database**: The launcher by default exposes the logged-in emails, and includes their IGN display name, access/session token, user ID, UUID and e-mail. (Example below)
* **Launcher version**: The launcher also exposes information about the build version. Information exposed includes the launcher build name, the format and the profiles format. (Examples below)
* **Launcher session information**: Such as the currently selected user UUID, the currently selected profile and the current client token.
* **Basic launcher features**: This includes fetching Minecraft's working directory. More features will be added to this.

* ****
# Examples

### Creating a custom profile:
```java
LauncherProfile myProfile = new LauncherProfileBuilder()
        .setName("My Profile")
        .setLauncherVisibility(LauncherVisibility.HIDE_AND_OPEN_ON_GAME_CLOSE)
        .setVersionID("1.8.8")
        .setIcon("Base64 encoded image here")
        // More setters are available, though we won't cover all of them
        .build();
```

### Profiles management:
1- Declaring the instance:
```java
// You can initiate your own profile manager as well if you want to provide a custom location for the launcher_version.json.
ProfileManager profileManager = ProfilesManager.getInstance();
```

2- For creating a profile
```java
// Add our own profile
LauncherProfile myProfile = new LauncherProfileBuilder()
        .setName("Boop")
        .setVersionID("1.8.8")
        .build();
profileManager.addProfile(myProfile).save();
// If we're done adding all the profiles we want, you must call:
```

3- For removing a profile
```java
profilesManager.removeProfile("Boop").save();
// If we're done removing all the profiles we want, you must call:
profileManager.apply(); // This will save all the changes done to the launcher
```

**Note**: ProfileManager allows chaining methods. For example, it's possible to do **profileManager.addProfile(profile).removeProfile("Profile to remove").save();**

### Authentication Database
The **Authentication Database** is available using **LauncherController**:

1- To declare our launcher controller instance:
```java
// You can initiate your own launcher controller as well if you want to provide a custom location for the launcher_version.json.
LauncherController controller = LauncherController.getInstance();
```

2- Access the authentication database:
```java
AuthenticationDatabase database = controller.getAuthenticationDatabase();
```

3- We can access all the profiles now
```java
ProfileUser user = database.getProfile("dfbd02a46be14153a203aed7204d35b6"); // This is for example my UUID
System.out.println("Display name: " + user.getDisplayName());
System.out.println("UUID: " + user.getUUID());
```

4- You can also get all logged in users
```java
List<ProfileUser> users = database.getProfileUsers();
for (ProfileUser user : users) {
    System.out.println("Display name: " + user.getDisplayName());
}
```

5- ...And the currently selected user:
```java
ProfileUser user = controller.getSelectedProfileUser();
...
```

### Launcher version
You can use this to, for example, check if the user is keeping up-to-date with the launcher versions.

Launcher version information are exposed through **LauncherController**:

1- To declare our launcher controller instance:
```java
// You can initiate your own launcher controller as well if you want to provide a custom location for the launcher_version.json.
LauncherController controller = LauncherController.getInstance();
```

2- Access the version:
```java
LauncherVersion version = controller.getLauncherVersion();
System.out.println("Launcher Build Name: " + version.getName());
```

### Launcher session information:
You can also access information such as the selected profile name, the selected user name, the client token, etc.