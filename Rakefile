task :dev do
  sh "bundle exec jekyll serve --livereload"
end

require 'fileutils'
require "image_processing/mini_magick"


task :thumbnails do
  originals_directory = File.expand_path('assets/images/art')
  originals = Dir.glob "#{originals_directory}/*.*"

  FileUtils.rm_rf("#{originals_directory}/thumbnails")
  FileUtils.mkdir("#{originals_directory}/thumbnails")
  thumbs_dir = File.expand_path('assets/images/art/thumbnails')

  originals.each do |file_path|
    filename = file_path.split('/').last
    thumbnail = ImageProcessing::MiniMagick
      .source(file_path)
      .strip
      .resize_to_fill(400, 400)
      .saver(quality: 85)
      .call(destination: "#{thumbs_dir}/#{filename}")
    
    puts "Created thumbnail for: #{filename}"
  end
end
