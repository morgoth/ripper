class Cd2mp3 < Thor
  namespace :ripper
  include Thor::Actions

  desc "rip DIR (defaults to '.')", "Rip cd to mp3"
  def rip(dir = ".")
    invoke :cdparanoia, dir
    invoke :lame, dir
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
        %x{lame #{file} #{mp3_file}}
      end
    end
  end

  desc "cleanup DIR (defaults to '.')", "Remove wav files"
  def cleanup(dir)
    inside(dir, :verbose => true) do
      Dir.entries(".").select { |e| e =~ /\.wav$/ }.each do |file|
        remove_file(file)
      end
    end
  end
end
