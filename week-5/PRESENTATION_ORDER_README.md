# Student Presentation Order Randomizer

This directory contains a Jupyter notebook that randomizes the order of student presentations for final project proposals. The notebook runs in a Podman container for consistency and reproducibility.

## Files

- `randomize_presentations.ipynb` - Jupyter notebook for randomizing presentation order
- `Spring_2026-STATS_roster.csv` - Student roster data
- `Dockerfile` - Container configuration for running Jupyter

## Prerequisites

- Podman installed on your system
  - macOS: `brew install podman`
  - Linux: Use your package manager (e.g., `apt install podman` or `dnf install podman`)
  - Windows: Download from [Podman website](https://podman.io/getting-started/installation)

## Instructions to Run

### Step 1: Navigate to the week-5 directory

```bash
cd week-5
```

### Step 2: Build the Podman image

```bash
podman build -t presentation-randomizer .
```

This command builds a container image named `presentation-randomizer` using the Dockerfile in the current directory.

### Step 3: Run the container

```bash
podman run -p 8888:8888 -v $(pwd):/app presentation-randomizer
```

**Explanation of the command:**
- `-p 8888:8888` - Maps port 8888 from the container to your local machine
- `-v $(pwd):/app` - Mounts the current directory to `/app` in the container (allows saving changes)
- `presentation-randomizer` - The name of the image to run

### Step 4: Access Jupyter Notebook

After running the container, you'll see output similar to:

```
[I 2026-04-28 21:59:30.123 ServerApp] Jupyter Server is running at:
[I 2026-04-28 21:59:30.123 ServerApp] http://0.0.0.0:8888/
```

Open your web browser and navigate to:

```
http://localhost:8888
```

### Step 5: Open and Run the Notebook

1. In the Jupyter interface, click on `randomize_presentations.ipynb`
2. To set a different random seed, modify the `RANDOM_SEED` variable in the second code cell
3. Run all cells by clicking **Cell → Run All** or pressing **Shift+Enter** on each cell
4. The final output will show a table with:
   - Presentation # (1-18)
   - Student Name
   - Student Email

### Step 6: Stop the container

When you're done, you have two options:

**Option 1: Stop from the same terminal (Recommended)**
- Go to the terminal where the container is running
- Press `Ctrl+C` (or `Cmd+C` on Mac)
- This will gracefully stop the Jupyter server and container

**Option 2: Stop from a different terminal**
```bash
# Find the running container
podman ps

# Stop it using the container ID or name
podman stop <container-id>
```

Example:
```bash
$ podman ps
CONTAINER ID  IMAGE                        COMMAND     CREATED        STATUS        PORTS                   NAMES
abc123def456  localhost/presentation-...   jupyter...  2 minutes ago  Up 2 minutes  0.0.0.0:8888->8888/tcp  amazing_name

$ podman stop abc123def456
```

## Changing the Random Seed

To generate a different presentation order:

1. Open the notebook in Jupyter
2. Find the cell with `RANDOM_SEED = 42`
3. Change `42` to any other number (e.g., `123`, `2026`, etc.)
4. Run all cells again

Using the same seed will always produce the same order, which is useful for reproducibility.

## Output

The notebook generates a clean table with three columns:
- **Presentation #**: The order number (1-18)
- **Name**: Student's full name
- **Email**: Student's email address

## Troubleshooting

### Port already in use
If port 8888 is already in use, you can use a different port:

```bash
podman run -p 8889:8888 -v $(pwd):/app presentation-randomizer
```

Then access Jupyter at `http://localhost:8889`

### Permission issues on Linux
If you encounter permission issues, you may need to run with `--userns=keep-id`:

```bash
podman run --userns=keep-id -p 8888:8888 -v $(pwd):/app presentation-randomizer
```

### Container won't start
Make sure Podman is running and you have built the image:

```bash
podman info  # Check Podman status
podman images  # Verify the image was built
```

## Alternative: Run without Podman

If you prefer to run locally without Podman:

1. Install required packages:
   ```bash
   pip install jupyter pandas numpy
   ```

2. Start Jupyter:
   ```bash
   jupyter notebook
   ```

3. Open `randomize_presentations.ipynb` in your browser

## Notes

- The notebook uses pandas and numpy for data manipulation and randomization
- The random seed ensures reproducibility - the same seed always produces the same order
- The CSV file must be in the same directory as the notebook