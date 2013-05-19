# Sample VN organisation

## Generale rules (or rather random thought dump)

    * lowercase letters only (+ digits if really required), no weird characters, underscores instead of spaces,
    * keep to the structure; if you think an asset doesn't really fit any of categories you can probably make a new directory for it in your branch and bring it up somewhere,
    * DRY, that is - Don't Repeat Yourself - if you think you'll be using something more than once it's probably good to put it into a separate name under a symbolic name (using Ren'Py's `define` directive), compare with how characters or music files are factored in their respective directories; if something comes up it's easier to change if it's only one place you have to change it in, than everywhere around the whole codebase,
    * with that in mind - use descriptive names for symbolic names, so that they will be unlikely to change,
    * that said, assets themselves would do good to have descriptive names too,
    * Ren'Py keeps everything in one global scope - that is regardless of file you define, say, a label or character, it is visible in all other files. Which means that when you use mundane name like "pick_up_cup" for a seemingly local thing such as a label be aware there may be some hardcore cup picking action in another route and kablam, you're fucked.
    Keeping that in mind it's a good idea to manually "namespace" the labels and whatnot, compare with sample start label names (`ER_Act1_Day1_Start`); in the case of factoring out common constants mentioned in the third bullet you can and should skip this "namespacing", since everyone is expected to use the same constant,
    * read Ren'Py documentation, it helps to have a vague idea of how this thing works.

## Assets

Assets are divided into these general categories:

    * art
    * audio
    * story

## Art

Art is so far divided into `characters` and `locations` directories, but that's just to give an example - I imagine more will probably be needed (like `sfx`, `event_cg` and whatnot).
Each folder is further subdivided into corresponding categories - in `characters` folder each character has his/hers own folder; in `locations` folder it's divided per logical location, say `bellforest`, `onboard_gekko` or something.
You can subdivide location further if you feel it's necessary, say `bellforest/school`, `bellforest/rentons_house`, `bellforest/rentons_house/garage/destroyed`, `bellforest/rentons_house/rentons_room`, `onboard_gekko/bridge`, `onboard_gekko/infirmary`, and so on.
I agree that this many nested folders may sounds somewhat uncommonsensey, but that's just to show what I have in mind with regard to assets grouping. In practice the rule of thumb should be: a folder grouping assets is a good idea if you have more than a couple of related CGs for this character/area (say, each location has a night and day version with some variations or something). You could even consider doing it for characters if there would be day and night versions of sprites.
It all depends of how many assets will you produce, find the point where having them in folders gives you much needed structure (it's easier to find a file if you know where to look than to sift through hundreds of files dumped in that one unlucky folder), but is not yet a hassle.

Ren'Py has a concept of tags for images - it looks for image most closely matching tags given in the script and uses it. 
Thusly asset filenames should begin with character's/location's/effect's/etc. name and then, separated with underscores, should follow asset tags. For example an image of a happy, carefree Eureka should be something like `art/characters/eureka/eureka_happy_carefree.png` and would be then referenced in the script with something like `show Eureka happy carefree`. A character image needs to be added to character description file to be usable in the script, as described in `Story` section.

## Audio

Similar rules govern audio files - group them if it makes sense, use descriptive names, et caetera.

Audio files do not have a similar tag system that images have. Instead I opted to put them in one file as `define`s to music file paths, so that when you decide that what is referred in script `renton_happy_go_lucky` music should be named differently it doesn't affect the script at all - one change in the `music.rpy` file and you're done.

## Story

This is fairly important how to structure it so I'd give it another level of headings.

In general Ren'Py doesn't care about game structure, but we do, to not end up with a one damn big brothel on fire. Or something.

### Characters

I don't feel that warrants another level of folder grouping - each character corresponds to a single `*.rpy` file where all related variables/image definitions/whatnot are kept. When you add a new character asset you should also update it in this file. For example when you add the aforementioned `art/characters/eureka/eureka_happy_carefree.png` asset, the line you would add in `eureka.rpy` would be `image Eureka happy carefree = "art/characters/eureka/eureka_happy_carefree.png"`. The fancy `im.Scale` in the example file is nothing to be concerned much about at the moment.

If you think that some grouping is in order, doing that with comments in the character file should be enough. Something in the vein of:

```
### A lot of crazy Anemone ###
image (...)
image (...)
image (...)

### Some sane Anemone ###
(...)
```

### Locations

Actually, it should follow the same rules character files follow. Just be sure to remember it's `image bg`. Also, read the fine Ren'Py manual.

### Script

Uh yeah, script.

I'm not exactly sure how exactly the script will be divided in parts, route first or act first or somehow else entirely, but that should be reflected in two places: the directory structure and the label names (it may affect some other syntactic elements since Ren'Py seems a little bit too global happy, but that will come out in the wash, I think).

There certainly should be folders for routes and acts. I'm not sure how to go from there, I guess that depends on how you writers want to structure the script - it could be a file per week, a file per day or a file per scene even. In case of file per scene it would do good to group them in folders by day or weeks or something.

Each Ren'Py scene should begin with a label of the format corresponding to directory structure. Say if we go route first and group by days it would be `label ER_Act1_Day1_Start` for Eureka/Renton route, act first, day first, start of the day, `label ER_Act1_Day1_MeetCute` if for some reason we would want to have Renton to clichely bump into Eureka the same day of script. The parts of label should be separated with underscores and part themselves should be CamelCase, that is with each word capitalized and then lumped together.

A scene can of course have more than one label - in case of choices and whatnot. It should be just another part of the label name, like `label ER_Act1_Day1_MeetCute_RentonIsVeryAwkwardlySorryForBumpingIntoEureka`. Long? Yes. But also descriptive and unique, as it should be.

If there will end up more than one scene in a file they should be separated from eachother in the manner mentioned in `Characters` section.

Anyway, script organisation needs discussing.

## Code

No divisions are in order yet. We will probably work something out as we go along. I imagine as soon as we figure consistent choice system, or replay system or whatnot, it will probably introduce some subfolders for that, where non-code people will be able to register choice flags and somesuch.

## HOW TO GIT?

Github and internets in general should have some description of how git works. Windows and Mac also have some nice Github-specific git clients. Why did I choose git? Well, git may be slightly mind-bending (even to programmers), but:
* svn sucks hard and is not an option you can seriously consider with a distributed team (also svn hosting sucks most of the time),
* not only script/code has to be backed up (Dropbox maybe would have been enough for that), but also the _history_. It's easy to delete a file forever on Dropbox or change the code so much you won't be able to get back to working state other than starting from scratch and git alleviates that,
* git has github which is the best collaboration tool since sliced bread. It has a pretty neat system of issues, pull requests and inline comments we can use to track what needs to be done. Also has a nice wiki so we can but all the titbits of information we need. There's even a nice tool called "huboard" which lets you nicely prioritise github issue in a sort of virtual cork board. Having a private repository requires some sort of subscription or an university email though,
* git lets us easily separate parts of script/code/assets that are in progress, from the parts that are ready to be tested, from the parts that are finalised, from the parts that already are released. It enables us to easily quality control what has been done so far and in future manage any bugfix releases and whatnot.,
* when you get past the initial learning curve using available awesome tutorials it turns out it only looks threatening - you only need to know `git add -A`, `git commit -m 'commit message'` and `git push` for the most use-cases - and even then you have the aforementioned dumbproof win&mac clients,
* `git stash` is awesome - you can stop doing one thing and work on another without polluting your workspace.


### Proposed process
Each major part of project should have it's own branch in git, say:
* master branch for game that is absolutely ready to release anytime,
* development branch, which is code that is the current probably-working-but-as-of-yet-untested state-of-art,
* testing branch, which is code ready to be tested, taken from development branch; tested and working code is then pushed into master,
* code branch, where people work on code,
* three branches for each route, where people work on script and art for that route,

Adding a new feature should go as follows:
* make a new feature branch from the branch you're working on, say `eureka_renton -> eureka_renton_roughing_out_act2`. Preferably only one person should work on that feature branch, but if you can manage merging eachother's work then no problem. If you're feeling really adventurous you can skip the feature branch thing entirely and just work on the main route branch, just be advised it may be more hassling,
* work on the feature,
* when you think you accomplished something noteworthy merge it up in the main route branch. You can use pull request to do that, or just talk about that with the rest of your route team.
* when the feature is merged into the main route branch make a pull request describing the changes and wait for the response if it fits nicely with current development branch or are there some changes that you would need to make; you can work on something else in the mean time, that's the beauty of git you don't get hold up waiting for review or never have to push half-ready work,
* if the feature it's good to go it's merged into development,
* development is regularly (say, once in a week, two or something) merged into testing, whereupon the game is tested if it's behaving acceptably,
* if it is, then the testing branch is synced to master and all the route branches so all the routes are on the same page,
* rinse and repeat.

Oh and DESCRIPTIVE COMMIT MESSAGES!

Of course that's just an idea that can and should be discussed, but everytime someone tries to get away with not using version control he is reminded it's a bad idea among cries and gnashing of teeth. So I strongly recommend to grit teeth and pull through that so we won't feel sorry in the future.
