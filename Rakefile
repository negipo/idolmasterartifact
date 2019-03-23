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
task 'cut' do |task, args|
  sh 'ffprobe -hide_banner data/out.mp4 2>data/info'
  duration_text = /Duration: ([\d:]+)/.match(open('data/info').read)[1]
  durations = duration_text.split(':')
  duration = durations[0].to_i * 3600 + durations[1].to_i * 60 + durations[2].to_i
  sh "ffmpeg -i data/out.mp4 -ss #{rand(duration - 3)} -t 3 data/cut.avi"
end

desc '#'
task 'glitch' do |task, args|
  avi = AviGlitch.open('data/cut.avi')
  avi.glitch(:keyframe) do |data|
    data.gsub(/\d/, '0')
  end
  avi.output('data/glitched.avi')
  sh 'ffmpeg -i data/glitched.avi data/glitched.mp4'
end
