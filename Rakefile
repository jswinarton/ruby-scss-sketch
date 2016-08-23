require 'fileutils'
require 'pathname'
require 'pry'
require 'webrick'


namespace :build do
  desc 'Copy static files to build dir'
  task :static do
    src_dir = Pathname.new('src').expand_path
    build_dir = Pathname.new('build').expand_path

    # ensure the output directory exists before writing
    FileUtils.mkdir_p build_dir

    ignore_paths = ['style']

    Dir.foreach(src_dir) do |item|
      next if item == '.' or item == '..' or ignore_paths.include? item
      src_file = src_dir.join(item)
      FileUtils.cp(src_file, build_dir)
    end
  end

  desc 'Compile sass files into css'
  task :sass do
    src_file = 'src/style/style.scss'
    build_file = Pathname.new 'build/style/style.css'

    # ensure the output directory exists before writing
    FileUtils.mkdir_p build_file.dirname

    sh "bundle exec sass #{src_file}:#{build_file}"
  end

  task :all => [:static, :sass]
end

desc 'Build everything'
task :build => 'build:all'

task :server do
  server = WEBrick::HTTPServer.new Port: 8000, DocumentRoot: File.expand_path('./build')
  trap 'INT' do server.shutdown end
  server.start
end
