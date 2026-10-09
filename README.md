# DevOps Notes

Personal knowledge base for my DevOps learning journey. Every hands-on lab I do is documented here: what the lab is, what broke, how I troubleshot it, and what I learned.

The folder is an [Obsidian](https://obsidian.md) vault, so notes link to each other with `[[wiki-links]]`.

## Structure

| Folder | Content |
|---|---|
| `Labs/` | Hands-on labs: goal, setup, the challenge, symptom, root cause, fix |
| `Linux/` | Linux commands, concepts and cheat sheets |
| `Troubleshooting/` | Problems and errors I hit, with diagnosis steps and fixes |

## Lab note format

Each lab follows the same template:

1. **Goal**: what the lab is about
2. **Setup**: commands to prepare the environment
3. **Challenge**: what was broken or changed
4. **Symptom**: what I observed
5. **Root cause**: why it happened
6. **Fix**: commands that solved it, plus verification
7. **Key takeaways**: what to remember

Troubleshooting notes are linked back to the lab they came from.

## Labs so far

| Lab | Topic |
|---|---|
| [NetworkManager - Modify Connection Not Applied](Labs/NetworkManager%20-%20Modify%20Connection%20Not%20Applied.md) | `nmcli con mod` edits the profile only; apply it with `con up` or `dev reapply` |

## Workflow

```bash
# do the lab, then document it
git add .
git commit -m "Add <lab name> lab and troubleshooting note"
```

## Environment

- Ubuntu with NetworkManager
- Labs run locally (dummy interfaces, containers, VMs) so nothing touches production
