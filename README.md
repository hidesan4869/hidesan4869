<p align="left">
  <a href="https://github.com/hidesan4869/hidesan4869/">
    <img src="https://komarev.com/ghpvc/?username=hidesan4869" alt="hidesan4869" />
  </a>
  <a href="http://twitter.com/hidesan4869">
    <img height="20" src="https://img.shields.io/twitter/follow/hidesan4869?label=Twitter&logo=twitter&style=flat" />
  </a>
  <a href="https://github.com/hidesan4869">
    <img height="20" src="https://img.shields.io/github/followers/hidesan4869?label=follow&logo=github&style=flat" />
  </a>
  <a href="https://www.reddit.com/user/hidesan4869">
    <img height="20" src="https://img.shields.io/reddit/user-karma/combined/hidesan4869?label=Reddit&logo=reddit&style=flat" />
  </a>
  <a href="https://stackoverflow.com/users/5720201/hidesan4869">
    <img height="20" src="https://img.shields.io/stackexchange/stackoverflow/r/5720201?label=StackOverflow&logo=stack-overflow&style=flat" />
  </a>
  <a href="http://qiita.com/hidesan4869">
    <img height="20" src="https://qiita-badge.apiapi.app/s/hidesan4869/posts.svg" />
  </a>
  <//qiita.com/hidesan4869">
    <img height="20" src="https://qiita-badge.apiapi.app/s/hidesan4869/contributions.svg" />
  </a>
</p>

  
[![trophy](https://github-profile-trophy.vercel.app/?username=hidesan4869&rank=S,AAA,AA,A,B,C&theme=dracula)](https://github.com/hidesan4869/github-profile-trophy)
  
[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=hidesan4869&layout=compact&theme=radical)](https://github.com/anuraghazra/github-readme-stats)
![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=anuraghazra&count_private=true&theme=radical)

name: Metrics

on:
  repository_dispatch:
    types: [test_trigger]
  workflow_dispatch:
  schedule:
    # Runs at 4am JST
    - cron: '30 19 * * *'

jobs:
  github-metrics:
    name: Update Readme with Metrics
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: vn7n24fzkq/github-profile-summary-cards@release
        env:
          GITHUB_TOKEN: ${{ secrets.GH_TOKEN }}
        with:
          USERNAME: ${{ github.repository_owner }}
      - uses: anmol098/waka-readme-stats@master
        with:
          WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
          GH_TOKEN: ${{ secrets.GH_TOKEN }}
          SHOW_PROJECTS: "False"
          SHOW_PROFILE_VIEWS: "False"
      - uses: lowlighter/metrics@latest
        with:
          token: ${{ secrets.GH_TOKEN }}
          plugin_reactions: yes
          plugin_reactions_limit: 200                 # Compute reactions over last 200 issue comments
          plugin_reactions_days: 14                   # Compute reactions on issue comments posted less than 14 days ago
          plugin_reactions_details: count, percentage # Display reactions count and percentage
          plugin_reactions_ignored: bot               # Ignore "bot" user
          plugin_achievements: yes
          plugin_achievements_threshold: B       # Display achievements with rank B or higher
          plugin_achievements_secrets: yes       # Display unlocked secrets achievements
          plugin_achievements_ignored: octonaut  # Hide octonaut achievement
          plugin_achievements_limit: 0           # Display all unlocked achievement matching threshold and secrets params
          plugin_wakatime: yes
          plugin_wakatime_token: ${{ secrets.WAKATIME_API_KEY }}      # Required
          plugin_wakatime_days: 7                                   # Display last week stats
          plugin_wakatime_sections: time, projects, projects-graphs # Display time and projects sections, along with projects graphs
          plugin_wakatime_limit: 4                                  # Show 4 entries per graph
          plugin_wakatime_url: https://wakatime.com                  # Wakatime url endpoint
          plugin_wakatime_user: .user.login                         # User
          plugin_topics: yes
          plugin_topics_sort: stars    # Sort by most starred topics
          plugin_topics_mode: mastered # Display icons instead of labels
          plugin_topics_limit: 0       # Disable limitations
      # - uses: avinal/Profile-Readme-WakaTime@master
      #   with:
      #     WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
      #     GITHUB_TOKEN: ${{ secrets.GH_TOKEN }}
      # - uses: athul/waka-readme@master
      #   with:
      #     WAKATIME_API_KEY: ${{ secrets.WAKATIME_API_KEY }}
  update-readme-with-blog:
    name: Update this repo's README with latest blog posts
    needs: github-metrics
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Pull in blog posts
        uses: gautamkrishnar/blog-post-workflow@master
        with:
          feed_list: "https://zenn.dev/yutakatay/feed"
