FROM python:3.13.11-slim
# from means you copy from a docker image, which contains uv and put in /bin/
# so uv will be available in docker after execution
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# RUN pip install pandas pyarrow

WORKDIR /code

# instead of using ENTRYPOINT ["uv", "run", "python", "pipeline.py"]
ENV PATH="/code/.venv/bin:$PATH"
# this is where the uv is intalled to create virtual env
# no need to add "uv" in the entrypoint
# when ENTRYPOINT ["python", "pipeline.py"], the python is from /bin

COPY pyproject.toml .python-version uv.lock ./

# to make sure the uv dependencies are the same
RUN uv sync --locked
# only for main dependencies, but not install the dev dependencies
# e.g. the --dev pgcli, the dev dependencies are dependencies needed for development
# we need pgcli to interact with development environment, but not in production, so we use --dev

# this is copy from the host to the docker image
COPY pipeline.py .

# uv → the executable
# "run" "python" "pipeline.py" → arguments passed to uv
# ENTRYPOINT ["uv", "run", "python", "pipeline.py"]
ENTRYPOINT ["python", "pipeline.py"]