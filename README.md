# Foreman Runit Template

The [foreman](https://github.com/ddollar/foreman) [runit](http://smarden.org/runit/) template erb files.

## Usage

```
# e.g. capistrano 3 foreman export task. (These erb files are in `shared/runit-template`)
namespace :foreman do
  desc "Export runit configuration scripts"
  task :export do
    on roles(:app) do |host|
      within release_path do
        execute :bundle, <<-CMD.gsub(/\s{2}/, "")
exec \
foreman export runit /path/to/service \
  -f ./Procfile \
  -a #{fetch(:application)} \
  -l #{shared_path.join("log")} \
  -u #{host.user} \
  -t #{shared_path.join("runit-template")}
        CMD
      end
    end
  end
end
```


## Change

There are two changes from [default template](https://github.com/ddollar/foreman/tree/master/data/export/runit).

### chpst -u

The user will be ignored if `sv` user is same with `-u #{host.user}` at runtime.  
(It runs just only command without `chpst -u USER`)

It may be useful for user specific runit service.

### svlogd -t

The `-t` is given to `svlogd`.  
This will add daemontools's [tai64](http://www.tai64.com/) format timestamp.


## Links

* [foreman manual](http://ddollar.github.io/foreman/)
* [foreman runit template](https://github.com/ddollar/foreman/tree/master/data/export/runit)
* [TAI64, TAI64N, and TAI64NA](http://cr.yp.to/libtai/tai64.html)


## License

MIT License
