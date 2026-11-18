# Using the skills

The skills in this folder form the plugin `bvleur-tech-projects`, in the marketplace of the same name. This page
says how to add them and how to bring a project in line with the standards they point to.

## Add the marketplace and the plugin

### claude.ai and Claude Desktop

1. Open **Customize** in the left sidebar, then the **Plugins** tab.
2. Click **Add**, then **Add marketplace**, then **Add from a repository**.
3. Enter `bvleur/bvleur-tech-projects`.
4. In the **Discover** tab, select the plugin **bvleur-tech-projects** and click **Add**.

The plugin is saved to your account. Claude Code (v2.1.273 or later, signed in with the same account) picks up your
account's plugins each time it starts.

### Claude Code on your machine

If the account sync does not bring the plugin, install it yourself:

```sh
claude plugin marketplace add bvleur/bvleur-tech-projects
claude plugin install bvleur-tech-projects@bvleur-tech-projects
```

`claude plugin marketplace update bvleur-tech-projects` fetches the newest version of the standards.

### In one project

To have the plugin in every session of a project, also for anyone else who clones it, add this to the project's
`.claude/settings.json` and commit it:

```json
{
  "extraKnownMarketplaces": {
    "bvleur-tech-projects": {
      "source": { "source": "github", "repo": "bvleur/bvleur-tech-projects" }
    }
  },
  "enabledPlugins": {
    "bvleur-tech-projects@bvleur-tech-projects": true
  }
}
```

`claude plugin marketplace add bvleur/bvleur-tech-projects --scope project` followed by
`claude plugin install bvleur-tech-projects@bvleur-tech-projects --scope project` writes the same settings.

### Claude Code in the cloud

Cloud sessions do not load plugins. They do read the project's `CLAUDE.md`, and every skill makes the project link to
its standard from there (see below), so Claude finds the standards through those links. A SessionStart hook that
fetches the standards at the start of a session is planned, in the `new-project` skill.

## Bring a project up to date

With the plugin on, open a Claude Code session in the project and ask:

> Bring this project in line with the bvleur-tech-projects standards.

The skills then:

1. compare the project with each standard and list where it differs;
2. add the links to the standards: a "Standards" section in `CLAUDE.md`, and a comment at the top of the file the
   standard is about (such as `mise.toml`);
3. make the changes on a branch and open a pull request, so you review and merge them.

A deliberate deviation stays, written in the project's `CLAUDE.md` with its reason.

When a standard changes here, run the same request again in each project. The links mean a project always points to
the current version; the request makes the project itself follow it.
