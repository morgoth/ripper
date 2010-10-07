class Cd2mp3 < Thor
  namespace :ripper
  include Thor::Actions

  desc "disk DIR (defaults to '.')", "Rip cd to mp3"
  def disk(dir = ".")
    invoke :cdparanoia, dir
    invoke :lame, dir
    invoke :cleanup, [dir, "wav"]
  end

  desc "wma DIR (defaults to '.')", "Convert wma to mp3"
  def wma(dir = ".")
    invoke :mplayer, dir
    invoke :lame, dir
    invoke :cleanup, [dir, "(wma)|(wav)"]
  end

  desc "cdparanoia DIR (defaults to '.')", "Rip cd to wav files"
  def cdparanoia(dir = ".")
    empty_directory(dir)
    inside(dir, :verbose => true) do
      %x{cdparanoia -B}
    end
  end

  desc "lame DIR (defaults to '.')", "Convert wav to mp3 by lame"
  def lame(dir = ".")
    inside(dir, :verbose => true) do
      Dir.entries(".").select { |e| e =~ /\.wav$/ }.sort.each do |file|
        mp3_file = file.split(".").first + ".mp3"
        %x{lame "#{file}" "#{mp3_file}"}
      end
    end
  end

  desc "mplayer", "Convert wma files to wav by mplayer"
  def mplayer(dir = ".")
    inside(dir, :verbose => true) do
      Dir.entries(".").select { |e| e =~ /\.wma$/ }.sort.each do |file|
        wav_file = file.split(".").first + ".wav"
        %x{mplayer -vo null -vc dummy -af resample=44100 -ao pcm:waveheader:file="#{wav_file}" "#{file}"}
      end
    end
  end

  desc "cleanup DIR EXTENSION (defaults to '.')", "Remove files with given extension"
  def cleanup(dir = ".", extension)
    inside(dir, :verbose => true) do
      Dir.entries(".").select { |e| e =~ /\.#{extension}$/ }.each do |file|
        remove_file(file)
      end
    end
  end
end
