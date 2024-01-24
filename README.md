In a minimal flask app, running `flask run --extra-files=` or using any other argument that internally uses `SeparatedPathType` results in `TypeError: super(type, obj): obj must be an instance or subtype of type`



1. Create a minimal flask application (follow [the Quickstart](https://flask.palletsprojects.com/en/3.0.x/quickstart/#a-minimal-application)).
2. Install flask==3.0.1 (this bug is not reproducible with 3.0.0 AFAIK)
3. Run `flask run --extra-files=a`

**Expected Behavior:**
The server should start up.

**Actual Behavior:**
The command raises a `Type Error` and exits with code 1

<details><summary>Full Traceback</summary>

```
Traceback (most recent call last):
  File ".../flask-cli-bug/..venv/bin/flask", line 8, in <module>
    sys.exit(main())
             ^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/flask/cli.py", line 1105, in main
    cli.main()
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 1078, in main
    rv = self.invoke(ctx)
         ^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 1686, in invoke
    sub_ctx = cmd.make_context(cmd_name, args, parent=ctx)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 943, in make_context
    self.parse_args(ctx, args)
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 1408, in parse_args
    value, args = param.handle_parse_result(ctx, opts, args)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 2400, in handle_parse_result
    value = self.process_value(ctx, value)
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 2356, in process_value
    value = self.type_cast_value(ctx, value)
            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 2344, in type_cast_value
    return convert(value)
           ^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/core.py", line 2316, in convert
    return self.type(value, param=self, ctx=ctx)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/click/types.py", line 83, in __call__
    return self.convert(value, param, ctx)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/flask/cli.py", line 861, in convert
    return [super().convert(item, param, ctx) for item in items]
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File ".../flask-cli-bug/..venv/lib/python3.11/site-packages/flask/cli.py", line 861, in <listcomp>
    return [super().convert(item, param, ctx) for item in items]
            ^^^^^^^
TypeError: super(type, obj): obj must be an instance or subtype of type
```
</details>

Environment:

- Python version: 3.11.6
- Flask version: 3.0.1

Output from `pip freeze`:
```
blinker==1.7.0
click==8.1.7
Flask==3.0.1
itsdangerous==2.1.2
Jinja2==3.1.3
MarkupSafe==2.1.4
Werkzeug==3.0.1
```
