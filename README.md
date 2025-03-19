# ClipHit - Clipboard History Manager

ClipHit is a simple, lightweight clipboard history manager for macOS that allows you to access your clipboard history with ease.

![ClipHit Screenshot](https://via.placeholder.com/800x500/007aff/FFFFFF?text=ClipHit+Screenshot)

## Features

- Stores your clipboard history
- Quick access via global shortcut (Cmd+Shift+V)
- Search through your clipboard history
- Paste items with a single click
- System tray icon for easy access
- Persistent storage between app restarts

## Installation

### Via Homebrew (recommended)

#### Option 1: Using brew tap (for testing)

```bash
# Tap the repository
brew tap mohitgupta07/cliphit

# Install ClipHit
brew install cliphit
```

#### Option 2: Building from source

```bash
# Clone the repository
git clone https://github.com/mohitgupta07/cliphit.git
cd cliphit

# Install using Homebrew
brew install --build-from-source ./cliphit.rb
```

#### Option 3: Testing locally (without GitHub access)

If you need to test the Homebrew installation process locally without access to GitHub, follow these steps:

1. **Create a local formula file**

Create a test formula file called `cliphit_test.rb` with the following content:

```ruby
class CliphitTest < Formula
  desc "A clipboard history manager for macOS (test version)"
  homepage "https://github.com/mohitgupta07/cliphit"
  license "MIT"
  version "1.0.1"
  
  # Use HEAD for local development testing
  head do
    url "file:///path/to/your/cliphit/directory", :branch => "main" # Replace with your path and branch
  end
  
  def install
    # Create a mock app structure for testing
    app_dir = buildpath/"MockClipHit.app/Contents/MacOS"
    app_dir.mkpath
    (app_dir/"cliphit").write("#!/bin/bash\necho 'ClipHit Mock App'")
    system "chmod", "+x", "#{app_dir}/cliphit"
    prefix.install "MockClipHit.app"
    
    # Add a CLI executable
    bin.mkpath
    (bin/"cliphit").write("#!/bin/bash\necho 'ClipHit CLI'")
    system "chmod", "+x", "#{bin}/cliphit"
  end

  test do
    system "#{bin}/cliphit"
  end
end
```

2. **Install using the local formula**

```bash
# Install from the local formula (using HEAD to test from the current directory)
brew install --HEAD ./cliphit_test.rb
```

3. **Test the installation**

```bash
# Run the mock CLI
cliphit

# Verify installation path
brew --prefix cliphit_test
```

4. **Create a local tap (alternative method)**

If you want to set up a complete local tap:

```bash
# Create a local tap directory structure
mkdir -p ~/homebrew-cliphit/Formula

# Copy your formula file
cp cliphit_test.rb ~/homebrew-cliphit/Formula/cliphit.rb

# Tap the local directory
brew tap mohitgupta07/cliphit ~/homebrew-cliphit

# Install from the local tap
brew install --HEAD mohitgupta07/cliphit/cliphit
```

### Running from Source

```bash
# Clone the repository
git clone https://github.com/mohitgupta07/cliphit.git
cd cliphit

# Install dependencies
yarn install

# Start the app
yarn start
```

### Packaging the App

```bash
# Package the app for macOS
yarn package
```

After packaging, you can find the app in the `dist` folder. Drag it to your Applications folder to install.

## Usage

1. Launch ClipHit
2. The app will run in the background with an icon in your menu bar
3. Copy text as you normally would
4. Press `Cmd+Shift+V` to open the clipboard history window
5. Click on any item to paste it
6. Use the search bar to find specific items in your history

## Creating a Release

There are two ways to create a release:

### Option 1: Automated Release (Recommended)

The easiest way to create a release is to use the automated release script:

```
yarn release X.Y.Z
```

This command will:
1. Update version numbers across all files
2. Create a git tag and push it to GitHub
3. Trigger a GitHub Actions workflow that will:
   - Download the release archive from GitHub
   - Update the SHA256 hash in the Homebrew formula
   - Commit and push the changes automatically

This automated approach ensures the SHA256 in the formula is always accurate with zero manual intervention.

### Option 2: Manual Release Process

If you prefer more control, you can follow these manual steps:

1. Update the version in `package.json`
2. Commit the change: `git commit -am "Bump version to vX.Y.Z"`
3. Create and push a git tag: `git tag vX.Y.Z && git push origin vX.Y.Z`
4. The GitHub Actions workflow will automatically handle the rest

## GitHub Actions Workflow

This project uses GitHub Actions to automate the Homebrew formula updates:

1. When a new tag is pushed, the workflow is triggered
2. It downloads the release archive from GitHub
3. Calculates the SHA256 hash
4. Updates the formula with the correct hash
5. Commits and pushes the changes

You can monitor the progress of these workflows in the Actions tab of the repository.

**Note**: For the GitHub Actions workflow to function properly, you need to set up a `PERSONAL_ACCESS_TOKEN` secret in your repository settings with sufficient permissions to push to the repository. See [GitHub Token Setup Documentation](docs/github-token-setup.md) for detailed instructions.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Author

Mohit Gupta - [GitHub](https://github.com/mohitgupta07) 