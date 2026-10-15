# Skill Builder

Skill Builder is a Git-versioned workspace for creating, testing, and maintaining reusable agent skills. It keeps skill instructions and supporting resources in source control so changes can be reviewed, shared, and rolled back safely.

## Repository structure

Create each skill in its own directory:

```text
skills/
└── example-skill/
    ├── SKILL.md
    ├── scripts/       # Optional helper scripts
    ├── references/    # Optional supporting documentation
    ├── templates/     # Optional reusable templates
    ├── tests/         # Optional test cases or validation scripts
    └── assets/        # Optional templates or static files
```

The `SKILL.md` file is the entry point for a skill. It should clearly describe:

- What the skill does
- When the skill should be used
- The workflow the agent should follow
- Any required tools, inputs, or constraints
- How to verify a successful result

## Creating a skill

1. Create a directory under `skills/` using a short, descriptive name.
2. Add a complete `SKILL.md` with the skill's instructions.
3. Add only the scripts, references, and assets the skill needs.
4. Test the skill with representative requests and edge cases.
5. Commit the change with a focused message.

Example:

```sh
mkdir -p skills/my-skill
$EDITOR skills/my-skill/SKILL.md
git add skills/my-skill
git commit -m "feat: add my-skill"
```

## Git workflow

Use a branch for each skill or significant update:

```sh
git switch -c feat/my-skill
```

Keep commits small and descriptive. Suggested commit types include:

- `feat`: add a new skill or capability
- `fix`: correct skill behavior or instructions
- `docs`: improve documentation
- `refactor`: reorganize a skill without changing its behavior
- `test`: add or update validation scenarios

Before merging, review the diff and confirm that the skill contains no secrets, credentials, personal data, or machine-specific paths.

## Writing guidelines

- Make instructions explicit and actionable.
- Define when the skill applies and when it does not.
- Prefer short workflows with clear checkpoints.
- Keep reusable resources beside the skill that uses them.
- Document dependencies and expected outputs.
- Update the skill and its documentation in the same commit.

## License

No license has been specified yet. Add a `LICENSE` file before distributing skills outside this repository.
