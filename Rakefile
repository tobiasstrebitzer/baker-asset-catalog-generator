require 'fileutils'
require 'json'

task :generate_all do
  Rake::Task[:generate].invoke('AppIcon')
  Rake::Task[:generate].reenable
  Rake::Task[:generate].invoke('LaunchImage')
end

task :generate, [:image_set] do |_, args|
  image_set = args[:image_set]
  
  # Get image set extension
  if image_set == 'AppIcon'
    image_set_extension = 'appiconset'
  elsif image_set == 'LaunchImage'
    image_set_extension = 'launchimage'
  else
    image_set_extension = 'imageset'
  end
  
  # Define variables
  workspace_dir = "#{image_set}.#{image_set_extension}/"
  json_file = "#{image_set}.json"
  source_image_file = "#{image_set}.png"
  json_output_file = "#{workspace_dir}Contents.json"
  
  # Cleanup workspace
  FileUtils.rm_rf(workspace_dir) if File.directory?(workspace_dir)
  FileUtils.mkdir_p(workspace_dir)
  
  # Read json file
  json_data = File.open( json_file, "r" ) do |f|
    JSON.load(f)
  end
  
  # Loop through images
  json_data['images'].each do |image_spec|
    
    # Get size
    (width_str, height_str) = image_spec['size'].split('x')
    scale = image_spec['scale'].gsub('x', '').to_i
    width = width_str.to_i * scale
    height = height_str.to_i * scale
    size_max = [width, height].max
    base_width = width / scale
    base_height = height / scale
    
    # Get version
    if image_spec['minimum-system-version'].nil?
      version = 'ios56'
    elsif image_spec['minimum-system-version'] == '8.0'
      version = 'ios8'
    elsif image_spec['minimum-system-version'] == '7.0'
      version = 'ios78'
    else
      version = 'ios56'
    end
    
    # Generate filename
    filename_array = []
    filename_array.push(image_spec['idiom']) if image_spec['idiom']
    filename_array.push(image_spec['orientation']) if image_spec['orientation']
    filename_array.push(version)
    
    # Add subtype
    if image_spec['subtype']
      if image_spec['subtype'] == '736h'
        filename_array.push('retina-hd-55')
      elsif image_spec['subtype'] == '667h'
        filename_array.push('retina-hd-47')
      else
        filename_array.push(image_spec['subtype'])
      end
    end
    
    # Add extent
    filename_array.push(image_spec['extent']) if image_spec['extent']
    
    # Add size
    filename_array.push(image_spec['size'])
    
    if scale > 1
      filename = "#{filename_array.join('-')}@#{scale}x.png"
    else
      filename = "#{filename_array.join('-')}.png"
    end
    
    # Generate image
    cmd = "sips -Z #{size_max} -c #{height} #{width} #{source_image_file} --out #{workspace_dir}#{filename}"
    puts "- #{cmd}"
    system(cmd)
    
    # Update json data
    image_spec['filename'] = filename
    
  end
  
  # Save Contents.json
  File.open(json_output_file,"w") do |f|
    f.write(JSON.pretty_generate(json_data))
  end
  
  puts "- completed"
  
end

task default: %w[generate_all]