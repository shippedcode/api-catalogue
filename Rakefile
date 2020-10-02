desc "Build middleman site"
task :build do
  sh "bundle exec middleman build --clean --bail"
end

desc "Publish built site to Github pages"
task :publish do
  require "tmpdir"

  rev = `git rev-parse --short HEAD`.chomp

  publish_dir = ENV.fetch("CLONED_GH_PAGES_DIR") do
    tmp_dir = Dir.mktmpdir("publish-api-catalogue")
    repo_url = `git config --get remote.origin.url`.chomp
    sh("git clone --single-branch --branch gh-pages #{repo_url} #{tmp_dir}")
    tmp_dir
  end

  sh("rsync -a --delete --exclude .git build/api-catalogue/ #{publish_dir}")

  sh("git -C #{publish_dir} add --all")
  sh("git -C #{publish_dir} commit -m 'Publish #{rev}'") do |ok, _|
    if ok
      sh("git -C #{publish_dir} push")
    else
      puts "Nothing to commit, skipping push"
    end
  end
end
