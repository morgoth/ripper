class Ripper < Thor
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

  desc "video FILE", "Compress given video file (output with avi extension)"
  def video(file)
    new_filename = (file.split(".")[0..-2] << "avi").join(".")
    %x{mencoder #{file} -o #{new_filename} -ovc lavc -oac mp3lame}
  end

  desc "split DIR (defaults to '.')", "Split flac album file into separate ones"
  def split(dir = ".")
    inside(dir, :verbose => true) do
      # is there a better way to escape whitespace in file name?
      cue_file = Dir.entries(".").find { |e| e =~ /\.cue$/ }.gsub(" ", '\ ')
      flac_file = Dir.entries(".").find { |e| e =~ /\.flac$/ }.gsub(" ", '\ ')

      # split files
      %x{cuebreakpoints #{cue_file} | shnsplit -o flac -a ripper-split- #{flac_file}}

      say("Copying metatags")
      %x{cuetag #{cue_file} ripper-split-*.flac}

      say("Renaming files")
      Dir.entries(".").select { |f| f =~ /ripper-split/ }.sort.each do |file|
        artist = %x{metaflac #{file} --show-tag=ARTIST}.strip.gsub("ARTIST=", "")
        title = %x{metaflac #{file} --show-tag=TITLE}.strip.gsub("TITLE=", "")
        track_number = %x{metaflac #{file} --show-tag=TRACKNUMBER}.strip.gsub("TRACKNUMBER=", "")
        file_name = "#{track_number} - #{artist} - #{title}.flac"

        say("Renaming #{file} to #{file_name}")
        File.rename(file, file_name)
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
