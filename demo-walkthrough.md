# Cursor Demo Walkthrough

## 1. Navigating the Window

- Editor panes
- File system / explorer
- Markdown preview
- Agent AI pane
    - Model Selection
    - Planning mode
    - **VOICE MODE**

## 2. Extensions and tools

- Open Extensions (Ctrl+Shift+X)
- Install **Markdown Preview Enhanced**
- Ask an agent to install pandoc - **Essential for converting Markdown<>DOCX and others**
```can you please scan my system and attempt to install the pandoc app appropriately. ask me if you need help. if you already find it don't do anything just let me know```

## 3. Skills Files

- What is a skill?
- The `.cursor/skills/` folder
- Creating the slide deck from markdown

## 4. The perils of letting agents talk to the internet for you


## 5. Modifying a Skill

- Editing an existing skill
```Okay, I want to update my create slide deck skill to make Steyer branded presentations. So please use a fetch command to fully fetch the Steyer.net website and look at its HTML and CSS and then think hard and carefully update our skill file```

## 6. Advanced study

- Install the slide deck skill we've been working on:
```Can you please install the agent skill at this github page into our local environment appropriately? Verify that the license allows me to install and modify it, and then try learning the skill to verify installation when done? Give me any instructions needed for my environment at the end once you've attempted to install. https://github.com/Abernson/create-slide-deck-skill```
- Intialize a git repository to track your changes
```Please explain the basics of using git in VSCode/Cursor to me. You can ask me questions to determine my level of familiarity with the editor, git, and technical topics, and then give me precise targetted explanations and introduction to how to use the built in git features to track changes to text documents```
- Use pandoc to convert to DOCX, learn about terminal commands
```Please use my installed version of pandoc to convert this markdown to a docx file. Do the conversion for me, then explain to me how I can do the conversion myself in future using the terminal built into Cursor. Assume a non technical background```
