require 'time'
require 'fileutils'
require 'yaml'

###############################################################################
# Check Software
###############################################################################

if `which fswatch`.empty?
   puts '
   fswatch does not appear to be installed. We\'re using fswatch to help sync
   files to the Flywheel directories on any changes.

   For mac users: `brew install fswatch`
   '
   exit
end

###############################################################################
# Load configuration
###############################################################################

if ! File.file?(Dir.pwd + '/conf.yml')
   puts '
   Error: A conf.yml file was not found.
   '
   exit
end

@conf = YAML.load_file(Dir.pwd + '/conf.yml')

###############################################################################
# Task Definition
###############################################################################

task :push do
    get_rsync_push_commands(ENV['env']).each { |cmd| system cmd }
end

task :sync do
    commands = get_rsync_push_commands ENV['env']
    exec "fswatch -o . | while read f; do #{ commands.join " && " }; done"
end

task :pull do
    commands = get_rsync_pull_commands ENV['env']
    commands.each { |cmd| system cmd }
end

###############################################################################
# Lib
###############################################################################

def get_rsync_pull_commands(env)
    @conf['sync'][env]['paths'].map do |name, path_conf|
        "rsync #{path_conf['rsync']['pull_flags']} #{get_rsync_excludes(path_conf['rsync']['ignore'])} #{path_conf['target']}/ #{path_conf['source']}"
    end
end

def get_rsync_push_commands(env)
    @conf['sync'][env]['paths'].map do |name, path_conf|
        "rsync #{path_conf['rsync']['push_flags']} #{get_rsync_excludes(path_conf['rsync']['ignore'])} #{path_conf['source']}/ #{path_conf['target']}"
    end
end

def get_rsync_excludes(ignores)
    if ignores.nil?
        return
    end
     # Create a set of exclude flags we'll pass to rsync to filter 
    filters = ignores 
        .map { |ignore| "--exclude '#{ignore}'"}
        .join(' ')
end
