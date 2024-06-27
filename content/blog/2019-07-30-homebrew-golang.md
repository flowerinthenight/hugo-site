---
title: "Using Homebrew for distributing Go (golang) apps"
description: "2019-07-30"
date: "2019-07-30"
paige:
  feed:
    hide_page: true
tags: [homebrew, go, golang, ruby]
weight: 1
---

For personal reference:

#### a) Create your own homebrew tap

Create a new GitHub public repository with a prefix `homebrew-`, i.e. `homebrew-tap`. This will house all the apps that you want to distribute via your tap. Users will install your apps using the following commands:

```sh
# No need to include the 'homebrew-' prefix
$ brew tap flowerinthenight/tap
$ brew install <toolname>
```

The `toolname` part will correspond to the filename inside your repository tap. If your repository has an entry with a filename of `toolx.rb`, it can be installed using the following commands:

```sh
$ brew tap flowerinthenight/tap
$ brew install toolx
```

Here's an example of a formula for a golang app:

```ruby
class Kubepfm < Formula
  desc "A simple wrapper to kubectl port-forward for multiple pods."
  homepage "https://github.com/flowerinthenight/kubepfm"
  url "https://github.com/flowerinthenight/kubepfm/archive/v0.0.3.tar.gz"
  sha256 "7852a5500f3e35a47b57d138c756de5641bc3c48bf7e329d2724c1107ccb1207"

  depends_on "go"

  def install
    ENV["GOPATH"] = buildpath
    ENV["GO111MODULE"] = "on"
    ENV["GOFLAGS"] = "-mod=vendor"
    ENV["PATH"] = "#{ENV["PATH"]}:#{buildpath}/bin"
    (buildpath/"src/github.com/flowerinthenight/kubepfm").install buildpath.children
    cd "src/github.com/flowerinthenight/kubepfm" do
      system "go", "build", "-o", bin/"kubepfm", "."
    end
  end

  test do
    assert_match /Simple port-forward wrapper tool for multiple pods/, shell_output("#{bin}/kubepfm -h", 0)
  end
end
```

You can check out [https://github.com/flowerinthenight/homebrew-tap](https://github.com/flowerinthenight/homebrew-tap) for reference.

The `url` part is the path of the source `tar.gz` of your source files. You can create this by using [tagged releases](https://help.github.com/en/enterprise/2.16/user/articles/about-releases) in GitHub.

You can generate the `sha256` part by running the `shasum` (OSX) or `sha256sum` (Linux) tool against your `tar.gz` file.

#### b) Updating your formula

If you have a new version of your tool, first, create a new tag or release. Download the new `tar.gz` file of the new release, run the `sha256sum/shasum` tool against it, and update the `.rb` file in your tap repository.

Example:

```sh
$ git clone https://github.com/flowerinthenight/kubepfm
$ cd kubepfm/
$ git tag v1.0.1 # our new tag
$ git push --tags # push tags to remote
$ wget https://github.com/flowerinthenight/kubepfm/archive/v1.0.1.tar.gz
# OSX
$ shasum -a 256 v1.0.1.tar.gz
7852a5500f3e35a47b57d138c756de5641bc3c48bf7e329d2724c1107ccb1207  v1.0.1.tar.gz
# Linux
$ sha256sum v1.0.1.tar.gz
7852a5500f3e35a47b57d138c756de5641bc3c48bf7e329d2724c1107ccb1207  v1.0.1.tar.gz
```

Finally, update the `url` and `sha256` part of your `.rb` file. Users will now be able to update their copies:

```sh
$ brew upgrade kubepfm
```

<br>

