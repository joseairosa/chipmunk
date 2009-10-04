# Rakefile added by John Mair (banisterfiend)

require 'rake/gempackagetask'
require 'rake/clean'

begin
    require 'rake/extensiontask'
rescue LoadError
    puts "rake-compiler not found...installing the rake-compiler gem..."
    `/bin/bash -l -c "gem install rake-compiler"`
    require 'rake/extensiontask'
    puts "...done!"
end

CHIPMUNK_VERSION = "4.1.0"

dlext = Config::CONFIG['DLEXT']

CLEAN.include("ext/**/*.#{dlext}", "ext/**/.log", "ext/**/.o", "ext/**/*~", "ext/**/*#*", "ext/**/.obj", "ext/**/.def", "ext/**/.pdb")
CLOBBER.include("**/*.#{dlext}", "**/*~", "**/*#*", "**/*.log", "**/*.o", "doc/**")


def apply_spec_defaults(s)
    s.name = "chipmunk"
    s.summary = "ruby bindings for the chipmunk physics engine"
    s.description = s.summary
    s.version = CHIPMUNK_VERSION
    s.author = "Scott Lembcke, Beoran, John Mair (banisterfiend)"
    s.email = 'jrmair@gmail.com'
    s.date = Time.now.strftime '%Y-%m-%d'
    s.require_path = 'lib'
    s.homepage = "http://code.google.com/p/chipmunk-physics/"
end


# common tasks
task :compile => :clean

# platform dependent tasks
if RUBY_PLATFORM =~ /darwin/

    spec = Gem::Specification.new do |s|
        apply_spec_defaults(s)
        s.platform = Gem::Platform::CURRENT
        s.extensions = FileList["ext/**/extconf.rb"]
        s.files = ["Rakefile", "README", "LICENSE", "lib/chipmunk.rb", "lib/1.8/chipmunk.#{dlext}", "lib/1.9/chipmunk.#{dlext}"] +
            FileList["ext/**/*extconf.rb", "ext/**/*.h", "ext/**/*.c"].to_a

    end

    Rake::ExtensionTask.new('chipmunk') do |ext|

        ext.lib_dir = "lib/#{RUBY_VERSION[0..2]}"              
        ext.config_script = 'extconf.rb'
        ext.config_options << 'macosx'
    end

    task :compile_multi => :clean do
        `/bin/bash -l -c "rvm 1.8.6,1.9.1 rake compile"`
    end

    task :gem => :compile_multi
    Rake::GemPackageTask.new(spec) do |pkg|
        pkg.need_zip = false
        pkg.need_tar = false
    end

else

    spec = Gem::Specification.new do |s|
        apply_spec_defaults(s)
        s.platform = Gem::Platform::RUBY
        s.extensions = FileList["ext/**/extconf.rb"]
        s.files = ["Rakefile", "README", "LICENSE", "lib/chipmunk.rb"] +
            FileList["ext/**/extconf.rb", "ext/**/*.h", "ext/**/*.c"].to_a
    end

    Rake::ExtensionTask.new('chipmunk', spec)  do |ext|
        ext.config_script = 'extconf.rb' 
        ext.cross_compile = true                
        ext.cross_platform = 'i386-mswin32'
    end

    Rake::GemPackageTask.new(spec) do |pkg|
        pkg.need_zip = false
        pkg.need_tar = false
    end
end
