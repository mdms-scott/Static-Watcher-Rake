require 'rubygems'
require 'haml'
require 'sass'
require 'fssm'

desc "watch"
task :watch do
  puts "watching for changes"

  FSSM.monitor do
    path './src/haml' do
      glob '**/*.haml'

      update { |b, r| compile_haml(b,r) }
      delete { |b, r| puts "HAML Delete in #{b} to #{r}" }
      create { |b, r| compile_haml(b,r) }
    end

    path './src/sass' do
      glob '**/*'

      update { |b, r| compile_sass(b,r) }
      delete { |b, r| puts "SASS Delete in #{b} to #{r}" }
      create { |b, r| compile_sass(b,r) }
    end
    
  end  
end

# Funcitons

def compile_haml(b,r)
  file = "#{b}/#{r}"
  puts "Compiling #{file} ... "
  content = File.new(file).read
  begin
    engine = ::Haml::Engine.new(content, (@options[:haml_options] || {}))
    output =  engine.render
  rescue StandardError => error
    puts "Error in HAML compilation of #{r}"
  end
  output_filename = r.gsub('.haml', '')
  output_file = "./public/#{output_filename}.html"
  FileUtils.mkdir_p File.dirname(output_file)
  File.open(output_file, 'w') { |f| f.write(output) }
  puts "done!"
end

def compile_sass(b,r)
  file = "#{b}/#{r}"
  output_split = r.split('.')
  output_filename = output_split[0]
  filetype = output_split[1]
  if filetype == "scss"
    sass_options = {:syntax => :scss}
  end
  puts "Compiling #{file} ... "
  content = File.new(file).read
  begin
    engine = ::Sass::Engine.new(content, (sass_options || {}))
    output =  engine.render
  rescue StandardError => error
    puts "Error in SASS compilation of #{r}"
  end
  output_file = "./public/stylesheets/#{output_filename}.css"
  FileUtils.mkdir_p File.dirname(output_file)
  File.open(output_file, 'w') { |f| f.write(output) }
  puts "done!"
end

