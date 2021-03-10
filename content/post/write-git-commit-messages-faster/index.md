---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Writing faster commit messages in Git"
subtitle: Using commit template files
summary: Using Git commit template files to reduce repetitive typing
tags: ["productivity", "git"]
categories: ['Productivity', 'Git']
date: "2021-03-10T12:58:23.371Z"
lastmod: "2021-03-10T12:58:23.371Z"
featured: false
draft: false
image:
  filename: featured.jpeg
  focal_point: Smart
  preview_only: false
---
Most software projects have a hook on the server that checks whether a ticket number is associated with a git commit,

```jsonc

    PROJ-123 Initial commit

```

If you are pair programming, you would also have your commit message with the pair name,

```jsonc

    PROJ-123 (John | Emily) Initial commit

```

In both these scenarios, there is some common part to the commit message that needs to be typed for every commit. Wouldn't it be nice if we could avoid this repetition?

Enter *Git commit template files* ✨

Steps:

1. Create a file `git-commit-template` at the root of your project and add the commit template message in it.

   You will most likely change this everyday, or even sooner if you are working on multiple tickets in a day.

   ```powershell

        PROJ-123 (Bhargav | Arun)

   ```
2. Add `git-commit-template` to `.gitignore`

   Different people in the team may have different commit templates at a given time. Template messages are personalised and need not be version controlled.
3. Commit using the template

   After making a few changes and adding them to the stage to be committed, run

   ```powershell

        git commit -t ./git-commit-template

    ```

   This will open the commit message editor in vim (default) with the commit message pre-populated from the template file.

   ![Commit message editor in vim](edit-commit-msg.png "Commit message edit screen")

   Edit the message, save and quit.

   If you are not familiar with vim, this [intro](https://flaviocopes.com/linux-command-vim/) will help you.

   **Hot tip**: When the message editor opens in vim, pressing `A` will take the cursor to the end of the line and start INSERT mode.

4. Add an alias to initiate quick commits

   All of this is great, but I don't want to type `git commit -t ./git-template` every time I want to make a commit. After all, the purpose of doing this is to reduce the repetitive typing!

   Add an alias `gc` for `git commit -t ./git-template`

   Reference on how to add aliases, [Windows](https://superuser.com/questions/560519/how-to-set-an-alias-in-windows-command-line) & [Mac and Linux / Unix](https://flaviocopes.com/how-to-set-alias-shell/).

   Now, every time you want to make a commit, you just have to type `gc`. That will open the message editor and you will only need to type the dynamic part of the commit message.

   Bhargav, what about when the ticket or pair changes?
   
   Just edit the template. This would usually be once a day. Not bad, eh?