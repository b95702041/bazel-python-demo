# Bazel Python Demo

A simple Python project demonstrating Bazel build system fundamentals with Jenkins CI/CD integration. Perfect for learning how Bazel works with Python code, dependencies, incremental builds, and automated pipelines.

## 🎯 What This Project Demonstrates

- **Basic Bazel concepts**: WORKSPACE, BUILD files, targets
- **Python with Bazel**: `py_library` and `py_binary` rules
- **Dependency management**: How modules depend on each other
- **Incremental builds**: Fast rebuilds when nothing changes
- **Cross-platform builds**: Works on Windows, macOS, and Linux
- **CI/CD Integration**: Automated builds with Jenkins
- **Containerized CI**: Jenkins running in Docker with automatic Bazel installation

## 📁 Project Structure

```
bazel-python-demo/
├── WORKSPACE           # Bazel project root (empty file)
├── MODULE.bazel        # Bazel module configuration
├── BUILD.bazel         # Build rules and targets
├── hello.py            # Main executable program
├── math_utils.py       # Reusable Python library
├── Jenkinsfile.bazel   # Jenkins CI/CD pipeline
├── .gitignore          # Ignore Bazel output directories
└── README.md           # This documentation
```

## 🚀 Quick Start

### Prerequisites

**Windows:**
```powershell
# Install Bazel using Chocolatey (run PowerShell as Administrator)
choco install bazel
```

**macOS:**
```bash
# Install Bazel using Homebrew
brew install bazel
```

**Linux (Ubuntu/Debian):**
```bash
# Install Bazel
sudo apt update
sudo apt install bazel
```

### Run the Demo

```bash
# Clone the repository
git clone https://github.com/b95702041/bazel-python-demo.git
cd bazel-python-demo

# Build and run the program
bazel run //:hello
```

**Expected output:**
```
Hello from Python with Bazel!
2 + 3 = 5
4 * 5 = 20
```

## 📚 Understanding the Code

### `math_utils.py` - Python Library
```python
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b
```
Simple math functions that can be reused by other modules.

### `hello.py` - Main Program
```python
from math_utils import add, multiply

def main():
    print("Hello from Python with Bazel!")
    print(f"2 + 3 = {add(2, 3)}")
    print(f"4 * 5 = {multiply(4, 5)}")

if __name__ == "__main__":
    main()
```
Imports and uses functions from `math_utils.py`.

### `BUILD.bazel` - Build Rules
```python
# Create a reusable Python library
py_library(
    name = "math_utils",
    srcs = ["math_utils.py"],
)

# Create an executable Python program
py_binary(
    name = "hello",
    srcs = ["hello.py"],
    deps = [":math_utils"],  # Depends on the math_utils library
)
```

**Key concepts:**
- `py_library`: Creates a reusable Python module
- `py_binary`: Creates an executable Python program
- `deps`: Declares dependencies between targets
- `name`: The target name (used with `bazel run //:hello`)
- `srcs`: Source files for this target

## 🔄 CI/CD Pipeline

This project includes a complete Jenkins CI/CD pipeline that demonstrates automated Bazel builds.

### Pipeline Stages

1. **🔧 Install Bazelisk** - Automatically installs Bazel using Bazelisk
2. **📥 Checkout** - Gets latest code from GitHub
3. **ℹ️ Bazel Info** - Shows Bazel version and configuration
4. **🧹 Clean Build** - Cleans previous build artifacts
5. **🏗️ Build All Targets** - Builds all Bazel targets
6. **🧪 Run Tests** - Executes all Bazel tests
7. **🚀 Run Application** - Executes the Python application
8. **🔍 Query Targets** - Shows all available Bazel targets

### Jenkins Setup

**Prerequisites:**
- Docker Desktop installed
- Jenkins running in Docker container

**Run Jenkins in Docker:**
```bash
docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --name jenkins jenkins/jenkins:lts
```

**Configure Jenkins Pipeline:**
1. Create new Pipeline job
2. Set Repository URL: `https://github.com/b95702041/bazel-python-demo.git`
3. Set Script Path: `Jenkinsfile.bazel`
4. Run the pipeline

### Pipeline Features

- ✅ **Automatic Bazel Installation** - Uses Bazelisk for easy setup
- ✅ **No Root Privileges Required** - Installs to user directory
- ✅ **Cross-Platform Compatible** - Works in Docker containers
- ✅ **Complete Build Lifecycle** - Clean, build, test, run
- ✅ **Error Handling** - Robust cleanup and error reporting

## ⚡ Bazel Performance

**First run** (cold build):
```
INFO: Elapsed time: 66.928s, Critical Path: 7.41s
```

**Second run** (cached build):
```
INFO: Elapsed time: 2.333s, Critical Path: 0.09s
```

This demonstrates Bazel's **incremental build** capability - it only rebuilds what has changed.

## 🛠️ Useful Bazel Commands

```bash
# Build everything in the project
bazel build //...

# Run a specific target
bazel run //:hello

# See the dependency graph
bazel query //...

# Get detailed build information
bazel query --output=build //:hello

# Clean build artifacts
bazel clean

# Check Bazel version
bazel --version
```

## 🔍 Exploring Further

### Add a Test
Create `math_utils_test.py`:
```python
import unittest
from math_utils import add, multiply

class TestMathUtils(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
    
    def test_multiply(self):
        self.assertEqual(multiply(4, 5), 20)

if __name__ == '__main__':
    unittest.main()
```

Add to `BUILD.bazel`:
```python
py_test(
    name = "math_utils_test",
    srcs = ["math_utils_test.py"],
    deps = [":math_utils"],
)
```

Run tests:
```bash
bazel test //:math_utils_test
```

### Understanding the MODULE.bazel File
The `MODULE.bazel` file (Bazel 6.0+) defines module dependencies and is the modern replacement for `WORKSPACE`. It provides:
- Module metadata
- Dependency declarations
- Version compatibility

## 🏗️ How Bazel Works

1. **Analysis Phase**: Bazel reads BUILD files and creates a dependency graph
2. **Execution Phase**: Bazel executes only the actions needed to build requested targets
3. **Caching**: Results are cached for fast incremental builds
4. **Parallelization**: Independent targets build in parallel

## 🤔 Why Use Bazel?

- **Fast builds**: Incremental and parallel builds
- **Reproducible**: Same input always produces same output
- **Multi-language**: Supports many programming languages
- **Scalable**: Works for small projects to massive codebases
- **Remote execution**: Can distribute builds across machines
- **CI/CD Ready**: Perfect for automated build pipelines

## 🔗 Related Projects

This project is part of a multi-repository CI/CD demonstration:

- **[simple-cicd-demo](https://github.com/b95702041/simple-cicd-demo)** - Node.js app with npm-based CI/CD
- **[bazel-python-demo](https://github.com/b95702041/bazel-python-demo)** - This Python app with Bazel-based CI/CD

Together, they demonstrate:
- Multi-language CI/CD pipelines
- Different build systems (npm vs Bazel)
- Jenkins integration with Docker
- Polyglot development workflows

## 📖 Learn More

- [Official Bazel Documentation](https://bazel.build/)
- [Bazel Python Tutorial](https://bazel.build/start/python)
- [BUILD File Reference](https://bazel.build/reference/be/overview)
- [Bazel Query Language](https://bazel.build/query/language)
- [Jenkins Pipeline Documentation](https://www.jenkins.io/doc/book/pipeline/)

## 🐛 Troubleshooting

**Common issues:**

1. **"bazel: command not found"**
   - Make sure Bazel is installed and in your PATH
   - In CI/CD, the pipeline automatically installs Bazelisk

2. **"No such package" errors**
   - Check that your BUILD.bazel file is in the right location
   - Verify target names match what you're trying to build

3. **Python import errors**
   - Make sure dependencies are declared in BUILD.bazel
   - Check that file names match exactly

4. **Permission errors on Windows**
   - Run PowerShell as Administrator when installing Bazel

5. **Jenkins pipeline failures**
   - Check that Bazelisk downloaded successfully
   - Verify PATH is set correctly in pipeline stages
   - Look for network connectivity issues

## ✅ Pipeline Status

**Latest Build:** ✅ SUCCESS

```
✅ Bazelisk: v1.26.0
✅ Bazel: 8.3.1
✅ Targets: 2 built successfully
✅ Tests: All passed
✅ Application: Executed successfully
```

## 🤝 Contributing

Feel free to:
- Add more math functions to `math_utils.py`
- Create additional Python modules
- Add tests for the functions
- Experiment with different Bazel rules
- Improve the Jenkins pipeline
- Add more CI/CD integrations

## 📝 License

This project is for educational purposes. Feel free to use and modify as needed.

---

**Next steps:** Try adding more Python files, creating tests, exploring Bazel's query capabilities, or integrating with other CI/CD tools!