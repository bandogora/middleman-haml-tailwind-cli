require 'thor/group'
require 'rbconfig'

module Middleman
  class Generator < ::Thor::Group
    include ::Thor::Actions
    source_root File.expand_path(File.dirname(__FILE__))

    def host_info
      arch = !(RbConfig::CONFIG['host_cpu'] =~ /arm/).nil? ? 'arm64' : 'x64'

      if !(RbConfig::CONFIG['host_os'] =~ /mswin|mingw|cygwin/).nil?
        "windows-#{arch}.exe"
      elsif !(RbConfig::CONFIG['host_os'] =~ /darwin|mac os/).nil?
        "macos-#{arch}"
      else
        "linux-#{arch}"
      end
    end

    def copy_default_files
      directory 'template', '.', exclude_pattern: /\.DS_Store$/
      twc_version = '3.3.6'

      bin = "tailwindcss-#{host_info}"

      %x(curl -sLO https://github.com/tailwindlabs/tailwindcss/releases/download/v#{twc_version}/#{bin})
      %x(chmod +x #{bin})
      %x(mv #{bin} tailwindcss)
      %x(./tailwindcss init)

      lines = File.readlines('tailwind.config.js')
      lines[2] = "  content: ['./source/**/*.{html,js,haml}']," << $/
      File.open('tailwind.config.js', 'w') { |f| f.write(lines.join) }
    end
  end
end
