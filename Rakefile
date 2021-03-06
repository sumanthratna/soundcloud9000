require 'bundler/gem_tasks'
require 'rake'
require 'yaml'

def dependencies(file)
  `otool -L #{file}`.split("\n")[1..-1].map { |line| line.split[0] }
end

task :dirs do
  begin
    Dir.mkdir 'vendor'
  rescue StandardError
    nil
  end
  begin
    Dir.mkdir 'vendor/bin'
  rescue StandardError
    nil
  end
  begin
    Dir.mkdir 'vendor/lib'
  rescue StandardError
    nil
  end
  begin
    Dir.mkdir 'vendor/dyld'
  rescue StandardError
    nil
  end
end

task ruby: :dirs do
  lines = `rvm info`.split("\n")[6..-1]
  ruby = YAML.safe_load(lines.join("\n"))

  version = ruby.keys.first
  home = ruby[version]['homes']['ruby']
  bin = ruby[version]['binaries']['ruby']
  # dylib = dependencies(bin)[0]

  `cp -a #{home}/lib/ruby/1.9.1/* vendor/lib`
  `cp #{bin} vendor/`
  # `cp #{dylib} vendor/dyld`
end

task libs: :dirs do
  %i[audite portaudio mpg123].each do |name|
    lib = `gem which #{name}`.chomp
    `cp #{lib} vendor/lib`
    if File.extname(lib) == '.bundle'
      dylib = dependencies(lib)[0]
      `cp #{dylib} vendor/dyld`
    end
  end
end

task package: %i[ruby libs] do
  `cp soundcloud9000 vendor`
  `cp -a bin/* vendor/bin`
  `cp -a lib/* vendor/lib`
  `tar czf soundcloud9000.tgz vendor`
end
