To use plugins with **lowlighter/metrics**, you configure them directly in your GitHub Actions workflow file (`.github/workflows/metrics.yml`). Each plugin is enabled and customised via `plugin_*` options in the `with:` block. [github](https://github.com/lowlighter/metrics)

## Basic structure

```yaml
name: Metrics
on:
  schedule: [{ cron: "0 0 * * *" }] # daily at midnight
  workflow_dispatch:

jobs:
  github-metrics:
    runs-on: ubuntu-latest
    steps:
      - uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.METRICS_TOKEN }}
          filename: metrics.svg
          base: header, activity, community, repositories
          config_timezone: Australia/Sydney

          # Enable plugins here
          plugin_languages: yes
          plugin_languages_ignored: html,css
          plugin_lines: yes
          plugin_followup: yes
          plugin_habits: yes
          plugin_achievements: yes
          plugin_achievements_threshold: C
```

## How to add a plugin

1. **Find the plugin name** in the [docs](https://github.com/lowlighter/metrics#-plugins) (e.g. `languages`, `habits`, `music`, `calendar`, `stargazers`). [github](https://github.com/lowlighter/metrics)
2. **Enable it** with `plugin_<name>: yes`.
3. **Customise it** using `plugin_<name>_<option>: value` (each plugin has its own options listed in its documentation page). [github](https://github.com/lowlighter/metrics/blob/master/.github/readme/partials/documentation/setup/action.md)

### Example: languages + habits + music

```yaml
- uses: lowlighter/metrics@latest
  with:
    token: ${{ secrets.METRICS_TOKEN }}
    filename: metrics.svg

    # Core
    base: header, activity, community, repositories

    # Languages plugin
    plugin_languages: yes
    plugin_languages_ignored: html,css
    plugin_languages_colors: github
    plugin_languages_limit: 8

    # Habits plugin
    plugin_habits: yes
    plugin_habits_days: 14
    plugin_habits_facts: yes

    # Music plugin (Spotify/Last.fm)
    plugin_music: yes
    plugin_music_provider: spotify
    plugin_music_playlist: https://open.spotify.com/embed/playlist/YOUR_PLAYLIST_ID
```

## Token & permissions

- You need a **personal access token** stored as `METRICS_TOKEN` in your repo secrets. [dev](https://dev.to/arcibyte/automate-your-github-readme-with-custom-svg-metrics-and-github-actions-21bd)
- Some plugins require extra scopes (e.g. `read:org` for org-related plugins, `repo` for private repos). Each plugin’s docs list required scopes. [github](https://github.com/lowlighter/metrics)

## Preview & generate config

Use the interactive configurator at **[metrics.lecoq.io](https://metrics.lecoq.io)** to:

- Toggle plugins
- Adjust options
- Preview the result
- Copy the generated workflow YAML directly [github](https://github.com/lowlighter/metrics/blob/master/.github/readme/partials/documentation/setup/action.md)
