# Checkbox

**Checkbox** is a minimal CLI tool for managing daily routines. It tracks which goals you've completed each day using a single JSON file. There is no GUI, no scheduling system — just simple daily tracking from the terminal.

---

## How It Works

Checkbox stores all routine data in a single JSON file.

* Each goal is **defined globally** and applies to **every day**.
* There is **no support for assigning goals to specific days of the week**.
* Only **checked** items are recorded per day.
* If nothing is checked on a given day, that date is not saved in the file.
* The file is fully portable and editable by hand if needed.

### Example JSON

```json
{
  "routine": {
    "drink": "Drink 2L of water",
    "read": "Read 10 pages"
  },
  "25.07.2025": ["drink", "read"]
}
```

---

## Syntax Notes

* `<item>` can be **one or more item names**, separated by spaces.
* Use `*` to apply the operation to **all items**.
* Item names with **spaces** must be wrapped in **quotes**.
* The `-t` flag can be used with `check`, `uncheck`, and `status`. It accepts:

  * Specific date: `dd.mm.yyyy`
  * Relative offset:

    * `-t=0`: today (default)
    * `-t=-1`: yesterday
    * `-t=1`: tomorrow

---

## Commands

### Add

```bash
checkbox add <item> <description>
```

Adds a new routine item.

---

### Remove

```bash
checkbox remove <item>...
```

Removes one or more items from the routine and from all historical entries.

---

### Check / Uncheck

```bash
checkbox check <item>... [-t=<date|offset>]
checkbox uncheck <item>... [-t=<date|offset>]
```

Marks items as checked or unchecked on the given date.

---

### Status

```bash
checkbox status <item>... [-t=<date|offset>]
```

Returns `true` if **all listed items** are checked on the given date. Defaults to today.

---

### Routine List

```bash
checkbox routine
```

Lists all defined routine items:

```
"<item>" "<description>"
```

---

### Edit

```bash
checkbox edit <item> <new-description>
```

Updates the description of a routine item.

---

### Rename

```bash
checkbox rename <old-item> <new-item>
```

Renames an item and updates it in all history.

---

### Streak

```bash
checkbox streak <item>...
```

Shows how many consecutive days **all** listed items have been checked, up to today.

---

### Clear

```bash
checkbox clear
```

Deletes all data — both routine definitions and history.

---

### Custom Data File

```bash
checkbox --data=<path> <command> ...
```

Use a custom JSON file instead of the default.

Example:

```bash
checkbox --data=~/routine.json check drink
```

---

## Build

Checkbox uses CMake and has no external dependencies aside from [nlohmann/json](https://github.com/nlohmann/json), which is included as a single header file.

To build:

```bash
git clone https://github.com/aa-irsya/checkbox
cd checkbox
cmake . -B build
cd build
make
```

This will generate a `checkbox` binary in the `bin` directory.

---

## Example

```bash
checkbox add drink "Drink 2L of water"
checkbox add read "Read 10 pages"
checkbox check drink read
checkbox status drink read        # true
checkbox streak drink read        # 1
checkbox uncheck drink
checkbox status drink read        # false
checkbox remove drink read
```
