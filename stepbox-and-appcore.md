
# Working with app-core in the step-box

If you’re on this page you are probably trying to develop locally in app-core (i.e the repository for the main Smartsheet app) with your current step-box set up. [Here]() are some helpful steps and tips to get that working successfully.  

1. Make sure your step-box is up and working properly. If you don’t yet have that set up, follow instructions [here]() first.
    - tip: make sure your mutagen .yml file for syncing points to the correct path where your repos are
2. From there make sure you can access both the Smartsheet app (the first link under “Helpful Links” on your home dashboard at `{stepboxname}.smart.ninja`)
3. If you just want to play around in the app without any local changes, that’s it! If you have things you want to test locally, continue on.
4. Make sure you have a build.properties file in `app-core` repo. If not, copy text from the build.properties.sample and replace the slot name with your step-box name
5. Next you have to run a local build of app-core. First, ssh into your step-box:
    ```
    ssh dev-{steboxname}
    ```
7. Then run a build command . Here is the [wiki]() for various build commands you can run. `/opt/git/app-core/src/main/build/build.sh` is the start of the build command you want to run, and then if it’s your first time running we suggest adding `-f` to the end of the command, `-w` if it's not the first time running the command. If you are making changes locally, build.sh `-w` is a reliable command to run to see your changes reflected in the app. It takes around ~5 minutes to run each time, and then you can refresh the app page and see your changes reflected. A sample command at the root would be:
    ```
    /opt/git/app-core/src/main/build/build.sh -f
    ```
    - tip: running the `-u` build command is not recommended as it resets the DB and removes any user permissions you have
    - tip: if you get a whole bunch of errors relating to `bean` symbols it can't find, try using `-f -s` as the build arguments instead of just `-f`
    - tip: If your build is failing due to being unable to read a number of files in `src-gen`, you may need to make some updates to your mutagen.yml config for app-core in `rm-step-box-mutagen`:
   ```
   app-core:
      mode: two-way-resolved
      flushOnCreate: false
   ```
    - Then run `mutagen project terminate` followed by `DOCKER_HOST="ssh://<your step>" mutagen project start` to ensure mutagen has picked up the new config
