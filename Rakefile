task :default => [:cdparanoia, :lame]

desc "Rip cd to wav files"
task :cdparanoia do
  dir = ENV["dir"]
  raise ArgumentError.new("Provide dir variable") unless dir
  mkdir_p(dir)
  cd(dir) do
    #%x{cdparanoia -B}
    puts "cdparanoia"
  end
end

desc "Convert wav to mp3 by lame"
task :lame do
  dir = ENV["dir"]
  raise ArgumentError.new("Provide dir variable") unless dir
  cd(dir) do
    FileList["*.wav"].each do |file|
      mp3_file = file.split(".").first + ".mp3"
      #%x{lame #{file} #{mp3_file}}
      puts "lame #{file} #{mp3_file}"
    end
  end
end

desc "Remove wav files"
task :remove_wav do
  dir = ENV["dir"]
  raise ArgumentError.new("Provide dir variable") unless dir
  cd(dir) do
    Dir.entries(".").select { |e| e =~ /\.wav$/ }.each do |file|
      File.delete(file)
    end
  end
end
