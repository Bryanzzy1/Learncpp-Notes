# LearnCPP Notes

These are C++ notes that I took following the [learncpp.com](https://www.learncpp.com) curriculum, and I structured them for use in [Obsidian](https://obsidian.md).

Each chapter is a folder containing a chapter overview note and individual concept notes with frontmatter tags, aliases, and wikilinks connecting related concepts across chapters.

## Using in Obsidian

Download or clone this vault and open the `learncpp/` folder in Obsidian. Graph view and backlinks will work immediately after downloading, and the notes are designed so every concept links to related concepts from other chapters.

## How these notes are built

Each chapter starts as a raw export from Notion (a `.zip` file). The conversion is handled by a [Claude Code](https://claude.ai/code) skill. Drop a new chapter zip into `learncpp/`, then run `/new-learncpp-chapter`. The skill:

- Extracts and cleans the Notion export
- Splits the flat note into individual concept notes with proper frontmatter
- Decides a node/subnode hierarchy for the chapter's topics
- Wires cross-chapter wikilinks to existing notes from prior chapters
- Deletes the original flat file

The full Claude skill is at [new-learncpp-chapter.md](new-learncpp-chapter.md).
