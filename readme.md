# Brandography Wordpress

This repo provides tooling that helps you sync files changes to and from 
another directory on the system. The goal is to help ease working with
Flywheel in a controlled manner.

The overall idea is that 1) you can author code here in a controlled setting 
and 2) pull in any plugin updates from Flywheel and commit them.

# Required Software

1. Rake `gem install rake`
2. fswatch `brew install fswatch`

# Set up

Pull from Flywheel and check `conf.yml` to make sure the sync paths match your paths.

If you're using Flyweel Teams then you'll want to symlink your `Local Sites` folder to the `Local Sites (Brandography)` folder.
This will avoid having to change the `conf.yml` across multiple systems.
Replace {PATH} with the site name.
```
ln -s ~/Local\ Sites\ \(Brandography\)/{PATH}/ ~/Local\ Sites/
```

# Usage

```
rake pull env=local
```

This will pull all configured paths from the `local` environment.

```
rake push env=local
```

This will push all configured paths to the `local` environment.

```
rake sync env=local
```

This will watch your local files and push to the `local` environment on 
any changes. Requires `fswatch` to be installed.