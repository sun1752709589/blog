# ruby如何处理文件的压缩和解压(zip格式)
本文使用的是rubyzip包。讲解使用ruby怎么创建一个基本的压缩、如何压缩一个目录及如何读取一个压缩文件。

## gem包的安装
`add gem 'rubyzip' to you Gemfile`

`bundle install`

or

`gem install rubyzip`

## 基本压缩-如何将某几个想要的文件打到一个压缩包
遍历想要加到压缩包的文件，逐个加入zip包。
```ruby
require 'zip'

folder = "Users/me/Desktop/stuff_to_zip"
input_filenames = ['image.jpg', 'description.txt', 'stats.csv']

zipfile_name = "/Users/me/Desktop/archive.zip"

Zip::File.open(zipfile_name, Zip::File::CREATE) do |zipfile|
  input_filenames.each do |filename|
    zipfile.add(filename, File.join(folder, filename))
  end
  zipfile.get_output_stream("myFile") { |f| f.write "myFile contains just this" }
end
```

## 如何递归的压缩一个文件夹
将如下文件加入你的app/models/ext/zip_file_generator.rb
```ruby
require 'zip'

class ZipFileGenerator

  def initialize(input_dir, output_file)
    @input_dir = input_dir
    @output_file = output_file
  end

  def write
    entries = Dir.entries(@input_dir) - %w(. ..)

    ::Zip::File.open(@output_file, ::Zip::File::CREATE) do |zipfile|
      write_entries entries, '', zipfile
    end
  end

  private

  def write_entries(entries, path, zipfile)
    entries.each do |e|
      zipfile_path = path == '' ? e : File.join(path, e)
      disk_file_path = File.join(@input_dir, zipfile_path)
      puts "Deflating #{disk_file_path}"

      if File.directory? disk_file_path
        recursively_deflate_directory(disk_file_path, zipfile, zipfile_path)
      else
        put_into_archive(disk_file_path, zipfile, zipfile_path)
      end
    end
  end

  def recursively_deflate_directory(disk_file_path, zipfile, zipfile_path)
    zipfile.mkdir zipfile_path
    subdir = Dir.entries(disk_file_path) - %w(. ..)
    write_entries subdir, zipfile_path, zipfile
  end

  def put_into_archive(disk_file_path, zipfile, zipfile_path)
    zipfile.get_output_stream(zipfile_path) do |f|
      f.write(File.open(disk_file_path, 'rb').read)
    end
  end
end
```
如何调用：
```ruby
directory_to_zip = "/tmp/input"
output_file = "/tmp/out.zip"
zf = ZipFileGenerator.new(directory_to_zip, output_file)
zf.write()
```

## 如何读取zip文件中的信息
```ruby
Zip::File.open('foo.zip') do |zip_file|
  zip_file.each do |entry|
    puts "Extracting #{entry.name}"
    entry.extract(dest_file)
    content = entry.get_input_stream.read
  end
  entry = zip_file.glob('*.csv').first
  puts entry.get_input_stream.read
end
```

## 其他注意事项
默认情况下，如果指定位置已经有同名zip文件则rubyzip包不会覆盖，如果想要覆盖旧文件则需要添加如下配置：

`Zip.continue_on_exists_proc = true`

