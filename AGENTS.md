# Development and testing

- Use the Ruby and Node versions in `.tool-versions`, Bundler, and tmux.
- From the repository root, run `bundle install` when dependencies are missing.
- Start the test environment with `./bin/dev up`. Jekyll runs in a detached tmux
  session and survives the shell that started it. Development mode includes live
  reload, drafts, and future posts. The default URL is `http://localhost:9000`.
- Run `./bin/dev logs` and wait for Jekyll's server-ready message before browser
  testing. `./bin/dev status` shows whether the process is running or has exited;
  it does not guarantee HTTP readiness. Failed server output stays available in
  `logs`; restart with `./bin/dev down` followed by `./bin/dev up` after fixing it.
- Use `./bin/dev logs 150` for more output, or `./bin/dev attach` for the live
  terminal. Detach with Ctrl-b then d to leave the server running.
- `PORT=9001 ./bin/dev up` changes the HTTP port. Extra Jekyll options may follow
  `up`, for example `PORT=9001 ./bin/dev up --livereload-port 35730`. Each checkout
  has its own session, but concurrent checkouts need distinct HTTP and live-reload
  ports. Repeating `up` reuses the existing session; stop it before changing options.
- Use **agent-browser** as the default tool for browser testing. For example:

  ```sh
  agent-browser --session luca3m-test open http://localhost:9000
  agent-browser --session luca3m-test snapshot -i
  agent-browser --session luca3m-test screenshot /tmp/luca3m-test.png
  agent-browser --session luca3m-test close
  ```

  Inspect the affected pages and interactions, and take fresh snapshots after
  navigation or DOM changes before using element references. If agent-browser is
  missing, install the CLI and run `agent-browser install` for its browser.
- Run `bundle exec jekyll build` to verify build-related changes.
- When testing is finished, close your browser session and run `./bin/dev down`.
  This stops only this checkout's tmux session. Repeating `down` is safe.
