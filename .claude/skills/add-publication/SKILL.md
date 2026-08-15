---
name: add-publication
description: Add a new publication entry to _includes/publications-script.html. Use when the user wants to add a blog post, video, or PR to their publications list.
disable-model-invocation: true
allowed-tools: Read, Edit, Bash
---

# Add Publication

Add a new entry to the `publications` array in `_includes/publications-script.html`.

## Publication object schema

```
{
  title: string,                   // Publication title (may include HTML like <i>)
  description: string,             // Short 1-2 sentence summary shown on card
  detailed_description: string,    // Full description shown in modal; may use <br>, <ol><li>, etc.
  job: string,                     // Company/employer: "HashiCorp", "AWS", "Microsoft", etc.
  content_type: string,            // "blog", "video", or "pr"
  tags: [string],                  // Lowercase tag strings, e.g. ["terraform", "aws", "eks"]
  link: string,                    // Primary URL (Medium, hashicorp.com, YouTube, GitHub PR, etc.)
  code_link: string,               // (optional) GitHub repo URL
  video_link: string,              // (optional) Video URL if content_type is "video" but link differs
  folder: string,                  // Dir name under assets/publications/ used for the card image
  popup_folder: string,            // (optional) Dir name to use for the modal image if different from folder
  date: "YYYY-MM-DD"              // Publication date
}
```

## Instructions

### Step 1 — Gather content

The user will paste blog/video/PR content. Extract as many fields as possible:

- **title**: use the article title verbatim (preserve HTML like `<i>` if needed)
- **description**: write a concise 1-2 sentence summary (what it demonstrates and what assets are provided — code, screenshots, architecture diagrams)
- **detailed_description**: use the article's own intro/overview section if present; format with `<br><br>` for paragraph breaks and `<ol><li>` for numbered lists; do NOT use markdown
- **job**: infer from context (publication domain, author bio, Medium publication name)
- **content_type**: "blog" for articles, "video" for YouTube/talks, "pr" for GitHub PRs
- **tags**: extract 3-8 lowercase technology/topic tags relevant to the content; if `job` is "AWS", always include `"aws"` in the tags array
- **link**: primary URL
- **code_link**: GitHub repo link if mentioned
- **folder**: derive a kebab-case directory name, e.g. `hashicorp-terraform-eks-auto-mode`
- **date**: extract from article metadata if present

### Step 2 — Ask for missing required fields

If any of these are missing or unclear, ask the user before proceeding:

- `date` (if not found in the content)
- `link` (the publication URL)
- `job` (if company cannot be inferred)
- `folder` (if unclear — suggest a name and confirm)
- `code_link` (ask "Is there a companion GitHub repo? If so, paste the URL.")

Ask all missing questions in a single message rather than one at a time.

### Step 3 — Insert the entry

Read `_includes/publications-script.html`. Insert the new publication object **at the top** of the `publications` array (after `const publications = [`), since entries are ordered newest-first.

Format the entry to match the existing style exactly:
- 2-space indentation inside the array
- Each field on its own line
- No trailing comma on the last field
- Comma after the closing `}` of the object (since it precedes existing entries)
- String values use double quotes
- Arrays use `["tag1", "tag2"]` format on a single line if short, or multi-line if long

Example format:
```
  {
    title: "My New Publication",
    description: "Short description here.",
    detailed_description: "Full detail here.<br><br>More paragraphs.",
    job: "HashiCorp",
    content_type: "blog",
    tags: ["terraform", "aws"],
    link: "https://example.com/post",
    code_link: "https://github.com/GlennChia/repo",
    folder: "hashicorp-terraform-example",
    date: "2026-08-15"
  },
```

### Step 4 — Handle the image directory

Check whether `assets/publications/<folder>/` already exists. If it does not, create it with `mkdir -p assets/publications/<folder>` and inform the user to add `architecture.png` to that directory.

If `popup_folder` is set and differs from `folder`, do the same check and creation for `assets/publications/<popup_folder>/`.
