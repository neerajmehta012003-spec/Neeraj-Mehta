
name: Generate Snake Animation

on:
  schedule:
    - cron: "0 0 * * *" # runs once a day
  workflow_dispatch: {}
  push:
    branches:
      - main

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    steps:
      - name: Generate Matrix-Green Snake SVG
        uses: Platane/snk@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark&color_snake=#00FF41&color_dots=03,004d1a,006622,00b32d,00FF41
            dist/github-contribution-grid-snake.svg

      - name: Push SVG to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
