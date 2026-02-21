# 📊 DSA Progress Dashboard

> Auto-updates as you check off problems in your notes. Powered by **Dataview**.  
> ⚠️ Requires the [Dataview plugin](https://github.com/blacksmithgu/obsidian-dataview) to be installed and enabled.

---

## 🧠 Overall Progress

```dataviewjs
const DSA_FOLDER = "Obsidian/DSA Striver A2Z notes";

const pages = dv.pages(`"${DSA_FOLDER}"`);

let totalDone = 0;
let totalAll  = 0;

for (let page of pages) {
  const tasks = page.file.tasks;
  totalAll  += tasks.length;
  totalDone += tasks.filter(t => t.completed).length;
}

const pct    = totalAll > 0 ? Math.round((totalDone / totalAll) * 100) : 0;
const filled = Math.round(pct / 5);
const empty  = 20 - filled;
const bar    = "█".repeat(filled) + "░".repeat(empty);

dv.paragraph(
  `**Total Problems** \`${bar}\` **${pct}%**\n\n` +
  `| ✅ Solved | 🔴 Remaining | 📦 Total |\n` +
  `|-----------|--------------|----------|\n` +
  `| ${totalDone} | ${totalAll - totalDone} | ${totalAll} |`
);
```

---

## 📂 Progress by Step

```dataviewjs
const DSA_FOLDER = "Obsidian/DSA Striver A2Z notes";

// ✏️ Add a new line here each time you create a new topic folder
const steps = [
  { folder: "Arrays",           label: "Step 03 · Arrays" },
  // { folder: "Binary Search",   label: "Step 04 · Binary Search" },
  // { folder: "Strings",         label: "Step 05 · Strings" },
  // { folder: "Linked List",     label: "Step 06 · Linked List" },
  // { folder: "Recursion",       label: "Step 07 · Recursion & Backtracking" },
  // { folder: "Bit Manipulation",label: "Step 08 · Bit Manipulation" },
  // { folder: "Stacks Queues",   label: "Step 09 · Stacks & Queues" },
  // { folder: "Sliding Window",  label: "Step 10 · Sliding Window" },
  // { folder: "Heaps",           label: "Step 11 · Heaps" },
  // { folder: "Greedy",          label: "Step 12 · Greedy" },
  // { folder: "Binary Trees",    label: "Step 13 · Binary Trees" },
  // { folder: "BST",             label: "Step 14 · BST" },
  // { folder: "Graphs",          label: "Step 15 · Graphs" },
  // { folder: "DP",              label: "Step 16 · Dynamic Programming" },
  // { folder: "Tries",           label: "Step 17 · Tries" },
];

const rows = [];

for (let step of steps) {
  const path  = `${DSA_FOLDER}/${step.folder}`;
  const pages = dv.pages(`"${path}"`);

  let done  = 0;
  let total = 0;

  for (let page of pages) {
    total += page.file.tasks.length;
    done  += page.file.tasks.filter(t => t.completed).length;
  }

  const pct    = total > 0 ? Math.round((done / total) * 100) : 0;
  const filled = Math.round(pct / 10);
  const empty  = 10 - filled;
  const bar    = "█".repeat(filled) + "░".repeat(empty);
  const status = pct === 100 ? "✅" : pct > 0 ? "🟡" : "⬜";

  rows.push([status, step.label, `\`${bar}\` ${pct}%`, `${done} / ${total}`]);
}

dv.table(["", "Topic", "Progress", "Solved"], rows);
```

---

## 🔁 Problems Marked for Revision

```dataview
TASK
FROM "Obsidian/DSA Striver A2Z notes"
WHERE !completed AND contains(tags, "revisit")
SORT file.name ASC
```

---

## ✅ Recently Solved

```dataview
TASK
FROM "Obsidian/DSA Striver A2Z notes"
WHERE completed
SORT file.mtime DESC
LIMIT 10
```

---

## 🔥 Unsolved Problems — Hard

```dataview
TASK
FROM "Obsidian/DSA Striver A2Z notes"
WHERE !completed AND contains(tags, "hard")
SORT file.name ASC
```

---

## 📅 Solved Today

```dataviewjs
const today = dv.date("today");
const pages = dv.pages('"Obsidian/DSA Striver A2Z notes"');

let solved = [];

for (let page of pages) {
  for (let task of page.file.tasks) {
    if (task.completed && task.completion) {
      const d = dv.date(task.completion);
      if (d && d.toISODate() === today.toISODate()) {
        solved.push(`- ✅ ${task.text} — [[${page.file.name}]]`);
      }
    }
  }
}

if (solved.length === 0) {
  dv.paragraph("*No problems solved today yet. Let's go! 💪*");
} else {
  dv.paragraph(`**${solved.length} problem(s) solved today:**\n\n` + solved.join("\n"));
}
```

---

> 💡 **Tip:** When you create a new topic folder (e.g. `Binary Search`), just uncomment its line in the `steps` array above. Make sure the `folder` name matches exactly what you named it in Obsidian.