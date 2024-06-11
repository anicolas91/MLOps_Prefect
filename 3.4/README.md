# Deployment

## BEFORE v2.10.8:

### Using a work pool:
1. Initialize a project
    ```bash
    prefect project init
    ```

2. Create a work-pool on the UI
   
3. start a worker that polls your work pool (no need to specify -t really)
    ```bash
    prefect worker start -p my-pool -t process
    ```
4. deploy your flow:
    ```bash
    prefect deploy my-flow.py:say_hello -n 'my-deployment' -p my-pool
    ```
5. start a run of the flow from the CLI
   ```bash
   prefect deployment run Hello/my-deployment
   ```

## AFTER v2.19.4:
### Using the serve method:
1. Call the `serve` method on the main flow you are about to deploy.
   - This creates a deployment
   - It stays running to listen for flow runs for this deployment, if found it will executed asynchronously.
2. Add deployment options to the `serve` method, such as `name`, `corn`, `tags`, `description`, or `version`.
    ```bash
        main_flow.serve(
        name="nyc-taxi-deployment-serve",
        # cron="* * * * *", # every minute of every day
        tags=["testing", "tutorial","green-taxi","2023","serve"],
        description="trains a model of trip duration using nyc green taxi 2023 data",
        version="tutorial/serve",
        )
    ```
3. Run the python script.
   ```bash
   python orchestrate-serve.py
   ```
4. Start a run of the flow from the CLI
   ```bash
   prefect deployment run 'main-flow/nyc-taxi-deployment-serve'
   ```

#### NOTE 1:
When you rerun with the `cron` option, you will find an updated deployment in the UI that is actively scheduling work! 
Simply uncommen the `cron` bit and run again.
Stop the script in the CLI using CTRL+C and your schedule will be automatically paused.

#### NOTE 2: 
For runs to be executed, the `flow.serve` script must be actively running.

#### NOTE 3: 
Deploying flows through the serve method is a fast way to start scheduling flows with Prefect. However, if your team has more complex infrastructure requirements or you'd like to have Prefect manage flow execution, you can deploy flows to a work pool.

### Using a work pool:

1. Create a work pool (can also use UI for this):
   ```bash
   prefect work-pool create zoompool -t process
   ```
2. Start a worker that polls your work pool (no need to specify -t really)
   ```bash
   prefect worker start -p zoompool -t process
   ```
3. Call the `deploy` method on the main flow you are about to deploy.
4. Add deployment options to the `deploy` method such as `name` and `work_pool_name`. Note that for process it needs an actual remote source so just sync github with the latest file and use that as a source.
   ```bash
    main_flow.from_source(
        source="https://github.com/anicolas91/MLOps_Prefect.git",
        entrypoint="3.4/orchestrate-deploy.py:main_flow",
    ).deploy(
        name="nyc-taxi-deployment-deploy",
        work_pool_name="zoompool",
        tags=["testing", "tutorial","green-taxi","2023","deploy"],
        description="trains a model of trip duration using nyc green taxi 2023",
        version="tutorial/deploy",
    )
    ```
5. Run the python script.
   ```bash
   python orchestrate-deploy.py
   ```
6. start a run of the flow from the CLI
   ```bash
   prefect deployment run main-flow/nyc-taxi-deployment-deploy
   ```

#### NOTE 1:
Deployments for workers are configured with `deploy`, which requires additional configuration. A deployment created with `serve` cannot be used with a work pool.

#### NOTE 2:
The primary reason to use work pools is for dynamic infrastructure provisioning and configuration. For example, you might have a workflow that has expensive infrastructure requirements and is run infrequently. In this case, **you don't want an idle process running within that infrastructure**.

#### NOTE 3:
Prefect intentially replaces the `_` in function names with `-`, which explains why `main_flow` became `main-flow` in the last step. It's a feature not a bug. See [here](https://github.com/PrefectHQ/prefect/issues/6284).