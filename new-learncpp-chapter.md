A new LearnCPP chapter has been dropped as a `.zip` file inside `learncpp/`. Your job is to extract it, clean the filename, then convert it into a structured folder following the exact same pattern used for every other chapter.

**Permissions:** All file operations on `learncpp/**` and `.claude/**` are pre-approved in `.claude/settings.json`. `Remove-Item`, `Expand-Archive`, and `Rename-Item` are all pre-approved. Proceed through all steps without asking for permission.

## Step 1 — Extract the nested zip

List `learncpp/` and find the `.zip` file (the only item that is not a directory).

Run the following PowerShell — adapting the actual filename — to extract the outer zip into a temporary folder, then extract the inner zip from that folder into `learncpp/`, then delete both zips and the temp folder:

```powershell
# 1. Find outer zip
$outerZip = Get-ChildItem "C:\Users\zhong\Documents\Obsidian Vault\learncpp" -Filter "*.zip" | Select-Object -First 1

# 2. Extract outer zip to a temp dir
$tempDir = "C:\Users\zhong\Documents\Obsidian Vault\learncpp\_temp_extract"
Expand-Archive -Path $outerZip.FullName -DestinationPath $tempDir -Force

# 3. Find the inner zip inside the temp dir
$innerZip = Get-ChildItem $tempDir -Recurse -Filter "*.zip" | Select-Object -First 1

# 4. Extract inner zip directly into learncpp/
Expand-Archive -Path $innerZip.FullName -DestinationPath "C:\Users\zhong\Documents\Obsidian Vault\learncpp" -Force

# 5. Clean up both zips and the temp dir
Remove-Item $outerZip.FullName -Force
Remove-Item $tempDir -Recurse -Force
```

## Step 2 — Clean the markdown filename

The extracted `.md` file will have a name like `Chapter 8 — Something abc123xyz.md` — the chapter name followed by a space and a random alphanumeric string before the extension.

Run this PowerShell to strip the trailing random suffix and leave only the chapter name:

```powershell
$vaultLearn = "C:\Users\zhong\Documents\Obsidian Vault\learncpp"
$mdFile = Get-ChildItem $vaultLearn -Filter "*.md" | Select-Object -First 1
# Strip trailing space + alphanumeric junk (e.g. " a1B2c3")
$cleanBase = $mdFile.BaseName -replace '\s+[A-Za-z0-9]+$', ''
if ($mdFile.BaseName -ne $cleanBase) {
    Rename-Item -Path $mdFile.FullName -NewName "$cleanBase.md"
}
Write-Output "Renamed to: $cleanBase.md"
```

Verify the final name looks correct (e.g. `Chapter 8 — Arrays.md`) before continuing.

## Step 3 — Find the new file

List the contents of `learncpp/`. The new chapter is now the only item that is a plain `.md` file (not a directory). Read its full contents.

## Step 4 — Study the existing chapters

Before creating anything, read several notes from previous chapters to understand current cross-chapter wikilinks. Specifically:
- Read the chapter overview of the most recent completed chapter (e.g. `Chapter 6 — Operators.md`) to see which prior concepts it links to.
- Read 2–3 subnode notes (e.g. `Macros.md`, `List Initialization.md`, `constexpr Functions.md`) to understand the subnode pattern.
- Read 2–3 concept notes that are heavily cross-linked (e.g. `Initialization.md`, `Functions.md`, `Memory Model.md`) to know which concepts exist and what they cover.

This research ensures every link you create points to a real note and every cross-chapter connection is accurate.

## Step 5 — Identify topics and plan the hierarchy

Parse the note's `## Notes` section. Each top-level bold bullet (`- **Topic**`) is a separate topic note. Use your judgment to name each note clearly (e.g. "Increment and Decrement", not "++/--").

Then decide the **node / subnode hierarchy**:

- **Top-level concept nodes** (`up: "[[<Chapter Name>]]"`, no `subnode` tag): the main concepts of the chapter that stand alone as reference material.
- **Subnodes** (`up: "[[<Parent Concept>]]"`, tag `subnode`): topics that are variants, refinements, or sub-cases of another concept in the same chapter. Examples of subnode relationships:
  - A specific initialization syntax (List Initialization) → subnode of the general concept (Initialization)
  - A macro directive (Macros) → subnode of the tool it belongs to (Preprocessing)
  - A summary or reference table → subnode of the concept it summarizes
  - A narrower usage pattern of a function type → subnode of that function type

Parent concept notes must list their subnodes in `related`.

## Step 6 — Create the folder

Create a directory with the same name as the file (minus `.md`), e.g. `Chapter 7 — Something/`.

## Step 7 — Create the chapter overview note

Inside the new folder, create `<Chapter Name>.md` with:

**Frontmatter:**
- `tags`: relevant `cpp/` subtags + `concept`, `syntax`, `best-practice` (if applicable), `chapter`
- `aliases`: human name (e.g. `Scope and Linkage`) + short code (e.g. `Ch7`)
- `up: LearnCPP`
- `related`: wikilinks to every topic note you will create (both nodes and subnodes)

**Body:** copy the original content verbatim, but replace each top-level bold topic heading with a wikilink — e.g. `- **Topic**` becomes `- **[[Topic]]**`. Preserve all sub-bullets, tables, and code blocks exactly.

## Step 8 — Create individual topic notes

For each topic, create `<Topic Name>.md` inside the folder.

**Frontmatter:**
- `tags`: relevant `cpp/` subtags + `concept`, `syntax`, `best-practice` where appropriate; add `subnode` for subnodes
- `aliases`: key terms, synonyms, important identifiers from the topic
- `up`: `"[[<Chapter Name>]]"` for top-level nodes; `"[[<Parent Concept>]]"` for subnodes
- `related`: wikilinks to concepts this topic connects to — include both:
  - **Intra-chapter links**: other notes in this chapter (parent, sibling concepts, subnodes)
  - **Cross-chapter links**: existing notes from prior chapters that this topic builds on or contrasts with (e.g. `[[Initialization]]`, `[[Functions]]`, `[[Memory Model]]`, `[[Undefined Behavior]]`, `[[One Definition Rule]]`, `[[Header Files]]`, `[[constexpr]]`, `[[Translation Unit]]`, `[[static_cast]]`, etc.)

**Body:** clean, concise prose/tables/code — not a raw copy-paste. Distill the key rules and examples. Use headers for sub-sections when the topic has distinct parts. **Embed cross-chapter wikilinks inline in the body text** wherever a prior concept is referenced or contrasted — not just in frontmatter.

**Footer** (last line): `> Full coverage: [[<Chapter Name>]] → <Topic Name>`

## Step 9 — Delete the original flat file

Remove the original `learncpp/<Chapter Name>.md` file.

## Style rules (apply throughout)

- Wikilinks use `[[Note Name]]` or `[[Note Name|Display Text]]` for aliases.
- Tags use lowercase `cpp/kebab-case`.
- No comments in code blocks beyond what was in the original.
- Individual notes are self-contained references — a reader should not need the chapter overview to understand them.
- Cross-chapter links must point to notes that actually exist in the vault. Verify against what you read in Step 2 before linking.
