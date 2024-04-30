# Version Control Choice (GitHub)

For our version control, we decided to use **Git**, utilizing **GitHub with GitHub Desktop**, even though there are better alternatives to use with Unreal Engine in particular. We did not use GitHub LFS.

## Why GitHub?
We decided to use GitHub for two key reasons:
- all members of the tech team had at least some experience with it, with most of us (including me) having used it for years
- it was easily available and accessible on the IMGD lab computers (in FL222 and FLA21)

Additionally, Jay had already done work in Unreal with GitHub before, so he knew how to set up the project (which Milo then copied when we had to switch versions from 5.3 to 5.2).

Git is the most popular version system amongst developers, with GitHub being the most popular platform to use with it. This means that there is a wide variety of documentation available,  and that if we had any weird issues, we would have been able to find an answer quickly. It also didn't make much sense to choose a method of version control that nobody was familiar just for the sake of it; we wanted to use something that we knew how to use. We were able to easily teach the art team how to use it as well, due to our pre-existing knowledge of how the software works.

## Pros and Cons
### Pros
- familiarity with the system and the corresponding software allowed us to be more efficient
- easy to rollback changes and restore previous versions of files
- branch system meant we could all work simultaneously (while being concious of what files are being edited; see cons)
- GitHub desktop was available on lab machines, and was also just very easy to use with Unreal
- allows the project to be publicly visible on GitHub, which is nice for portfolios or resumes

### Cons
- Unreal stores data as binary files, making most merge conflicts _unresolvable_ (must select one file or the other, or repeat work. this happened plenty of times)
- Unreal must be closed and re-opened when pulling changes (editing files while the editor is open causes issues)
- file size limit of 100MB; we didn't run into this problem for our project, but easily could have
