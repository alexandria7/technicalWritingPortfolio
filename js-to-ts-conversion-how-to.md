# How to Migrate JavaScript files in work-repository to TypeScript

*Please note that this doc outlines only the migration from JS to TS, not the additional work of migrating a file from class-based to functional components, which is out of scope for this current project. If you would like to see instructions on how to update files to a functional component structure, please view **this** doc.*

If you are reading this doc, you likely have an old JavaScript file in the work-repository repository that you would like to migrate to TypeScript. The following are some steps you should take to do this successfully and following current best practices.

0. First determine if the file you would like to migrate is actually being called somewhere in the repo. Sometimes with these old JS files we find that they are actually stale and can just be deleted. If this is the case, submit a PR with the file removed and any other relevant deletions or code changes. See **this** example PR of an old JS file deletion.

If the file is actually being utilized, then we can move on to the next steps:

1. First run the corresponding flow in your simulator/emulator so you understand what the expected behavior in the file should be.

2. Then convert the file to be a .ts file instead of a .js file. You can either rename the current file or create a new .ts file that you will transfer over the new code and delete the .js file after (note that renaming the file will allow the linter to help you find incorrect or missing types). 

3. Update prop types if applicable. Note: if file is extending another file type, like FullscreenAlert, you will need to update the prop type to include the props from the extended file. Also remember any redux types that might need to be included (mapStateToProps, mapDispatchToProps, etc).

```
import { FullscreenAlert, FullscreenAlertProps } from ‘~/modals’;

interface OwnProps {
      ableToAccept: Boolean;
}

type Props = FullScreenAlertProps<OwnProps>
…
export default class ComponentName extends FullscreenAlert<Props> {…}
```

4. For all functions being called in the file, make sure data being passed in has a type assigned.
    - You might find that the data type expected is not the data type being received (downsides of JS!). In an effort to avoid casting ``any`` as a type if possible, look around to see if this function is being called anywhere else and what data is being expected. You might have to pass in some options for types, like ``truckType: Maybe<string | Truck>`` but if we can consolidate types, even better. See example **here** of how we created a cohesive type so our props had accurate typing.

5. Update any relevant test files! It is encouraged to also convert the corresponding test files to TypeScript. Run the tests and make sure everything is passing, updating where you need to (using correct dummy data, etc.). See this example test file conversion.

6. Again, run the program locally on your simulator/emulator to make sure the flow isn’t broken and your changes were successful.

7. Open a PR and tag your coworkers on Slack to review, then merge once approved! :grin:
