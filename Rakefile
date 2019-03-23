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

desc '#'
task :cut do |task, args|
  sh 'ffprobe -hide_banner data/out.mp4 2>data/info'
  duration_text = /Duration: ([\d:]+)/.match(open('data/info').read)[1]
  durations = duration_text.split(':')
  duration = durations[0].to_i * 3600 + durations[1].to_i * 60 + durations[2].to_i
  finished = false
  until finished do
    sh "ffmpeg -y -i data/out.mp4 -ss #{rand(duration)} -t 5 data/cut.avi"
    if File.size('data/cut.avi') > 300_000
      finished = true
    end
  end
end

desc '#'
task :glitch do |task, args|
  sh 'datamosh', 'data/cut.avi', '-o', 'data/glitched.avi'
  sh 'ffmpeg -i data/glitched.avi -ss 2 -t 1 -r 1 -f image2 data/glitched.jpg'
  if ENV['VIDEO']
    sh 'ffmpeg -y -i data/glitched.avi data/glitched.mp4'
  end
end
