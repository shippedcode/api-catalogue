desc "Build middleman site"
task :build do
  sh "bundle exec middleman build --clean --bail"
end

desc "Publish built site to Github pages"
task :publish do
  require "tmpdir"
  require "uri"

  publish_dir = Dir.mktmpdir("publish-api-catalogue")
  rev = `git rev-parse --short HEAD`.chomp
  repo_url = `git config --get remote.origin.url`.chomp

  if ENV.key?("CI")
    uri = URI.parse(repo_url)
    uri.userinfo = ENV.fetch("GITHUB_TOKEN")
    repo_url = uri.to_s
  end

  sh("git clone --single-branch --branch gh-pages #{repo_url} #{publish_dir}")
  sh("rsync -a --delete --exclude .git build/api-catalogue/ #{publish_dir}")
  sh("git -C #{publish_dir} add --all")
  sh("git -C #{publish_dir} commit -m 'Publish #{rev}'")
  sh("git -C #{publish_dir} push")
end
