name: Generate snake animation

on:
  schedule: # execute every 12 hours
    - cron: "* */12 * * *"  # Executes every 12 hours

  workflow_dispatch: # allows manual triggering from GitHub UI
  
  push:
    branches:
      - master  # Trigger when changes are pushed to the master branch

jobs:
  generate:
    permissions:
      contents: write  # Ensure write access to the repository contents
    runs-on: ubuntu-latest
    timeout-minutes: 5

    steps:
      - name: Generate snake.svg
        uses: Platane/snk/svg-only@v3  # This action generates the snake SVG
        with:
          github_user_name: ${{ github.repository_owner }}  # Set this to your GitHub username
          outputs: dist/snake.svg?palette=github-dark  # Specify output directory and color palette

      - name: Push snake.svg to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0  # Pushes the generated SVG to the 'output' branch
        with:
          target_branch: output  # The branch to push the image
          build_dir: dist  # The folder containing the generated SVG
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}  # GitHub token for authentication
