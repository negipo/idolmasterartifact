require 'bundler/setup'
Bundler.require

DATA_DIR= 'data/'
desc '#'
task :download, %i(name) do |task, args|
  sh 'youtube-dl',
    '-o', %!'#{File.join(DATA_DIR, 'out.mp4')}'!,
    '--restrict-filenames',
    '-f', %!'best[ext=mp4]/best'!,
    args[:name]
end
