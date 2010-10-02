desc "Rip cd to wav files"
task :cdparanoia, :dir do |t, args|
  mkdir_p(args.dir)
  cd(args.dir, :verbose => false) do
    %x{cdparanoia -B}
  end
end

desc "Convert wav to mp3 by lame"
task :lame, :dir do |t, args|
  cd(args.dir, :verbose => false) do
    FileList["*.wav"].each do |file|
      mp3_file = file.split(".").first + ".mp3"
      %x{lame #{file} #{mp3_file}}
    end
  end
end

desc "Remove wav files"
task :remove_wav, :dir do |t, args|
  cd(args.dir, :verbose => false) do
    FileList["*.wav"].each do |file|
      File.delete(file)
    end
  end
end
