# Sublime Text — Competitive Programming Setup on Ubuntu

A practical setup guide for using Sublime Text as a lightweight Competitive Programming (CP) editor on Ubuntu Linux.

## Features

- Sublime Text Stable installation
- C++17 compilation and execution
- `input.txt` and `output.txt` file-based I/O
- Custom Competitive Programming build system
- Reusable C++ snippets
- Fast workflow for Codeforces, CodeChef, AtCoder and other judges

## 1. Install Sublime Text Stable

### Add the official signing key

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg \
  | sudo tee /etc/apt/keyrings/sublimehq-pub.asc > /dev/null
```

### Add the Stable repository

```bash
echo -e 'Types: deb\nURIs: https://download.sublimetext.com/\nSuites: apt/stable/\nSigned-By: /etc/apt/keyrings/sublimehq-pub.asc' \
  | sudo tee /etc/apt/sources.list.d/sublime-text.sources
```

### Install Sublime Text

```bash
sudo apt-get update
sudo apt-get install sublime-text
```

Verify the installation:

```bash
subl --version
```

## 2. Install the C++ Compiler

Install `g++` and essential build tools:

```bash
sudo apt update
sudo apt install build-essential
```

Verify the compiler:

```bash
g++ --version
```

Check the available C++17 support:

```bash
g++ -std=c++17 -dM -E -x c++ /dev/null | grep __cplusplus
```

## 3. Create a CP Workspace

Create a dedicated directory for your practice problems:

```bash
mkdir -p ~/CP
cd ~/CP
```

Create a sample problem directory:

```bash
mkdir -p ~/CP/Practice
cd ~/CP/Practice
touch main.cpp input.txt output.txt
subl .
```

Recommended structure:

```text
CP/
└── Practice/
    ├── main.cpp
    ├── input.txt
    └── output.txt
```

Keep `input.txt` and `output.txt` in the same directory as the source file when using the build system below.

## 4. Configure the C++17 CP Build System

A custom build system compiles your C++ source, runs the resulting executable and redirects standard input/output through text files.

### Create the build system

1. Open Sublime Text.
2. Go to **Tools → Build System → New Build System**.
3. Replace the default content with the following configuration.
4. Save it as `CP-C++17.sublime-build` in the suggested `User` package directory.
5. Select **Tools → Build System → CP-C++17**.

### Build system configuration

```json
{
    "shell_cmd": "g++ -std=c++17 -O2 -Wall -Wextra \"$file\" -o \"$file_path/$file_base_name\" && \"$file_path/$file_base_name\" < \"$file_path/input.txt\" > \"$file_path/output.txt\"",
    "working_dir": "$file_path",
    "selector": "source.c++",
    "file_regex": "^(.*?):([0-9]+):([0-9]+): (.*)$"
}
```

### How it works

- `-std=c++17`: enables C++17.
- `-O2`: enables compiler optimizations.
- `-Wall -Wextra`: enables useful compiler warnings.
- `"$file"`: the current C++ source file.
- `input.txt`: supplies standard input.
- `output.txt`: receives standard output.
- `&&`: runs the executable only if compilation succeeds.

**Run the program:** Open your `.cpp` file and press `Ctrl+B`.

**Important:** This build system expects both text files to exist in the source directory. Compiler errors appear in the build panel. Runtime error messages written to standard error remain in the build output; standard output is redirected to `output.txt`.

## 5. Test the Build System

Add the following to `main.cpp`:

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int a, b;
    cin >> a >> b;

    cout << a + b << '\n';

    return 0;
}
```

Put this in `input.txt`:

```text
10 20
```

Press `Ctrl+B`. The expected content of `output.txt` is:

```text
30
```

## 6. Add a Competitive Programming C++ Snippet

Snippets save repetitive typing by inserting commonly used C++ templates.

### Open the snippets directory

In Sublime Text, choose:

**Tools → Developer → New Snippet...**

Replace the default snippet with the following:

```xml
<snippet>
    <content><![CDATA[
#include <bits/stdc++.h>
using namespace std;

using ll = long long;
using ull = unsigned long long;
using pii = pair<int, int>;
using vi = vector<int>;

#define all(x) (x).begin(), (x).end()

void solve() {
    $0
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t = 1;
    // cin >> t;

    while (t--) {
        solve();
    }

    return 0;
}
]]></content>
    <tabTrigger>cp</tabTrigger>
    <scope>source.c++</scope>
    <description>Competitive Programming Template</description>
</snippet>
```

Save it as `cp.sublime-snippet` in the `User` package directory.

### Use the snippet

1. Open a C++ file.
2. Type `cp`.
3. Press `Tab`.

The full CP template will be inserted. The cursor will be positioned inside `solve()`.

**Note:** Uncomment `cin >> t;` when the problem contains multiple test cases. Keep it commented for single-test-case problems.

## 7. Add Useful C++ Snippets

Create additional snippets using **Tools → Developer → New Snippet...** and save them in the same `User` package directory.

### Fast I/O — `fastio.sublime-snippet`

```xml
<snippet>
    <content><![CDATA[
ios::sync_with_stdio(false);
cin.tie(nullptr);
]]></content>
    <tabTrigger>fastio</tabTrigger>
    <scope>source.c++</scope>
    <description>Fast Input/Output</description>
</snippet>
```

### For Loop — `fori.sublime-snippet`

```xml
<snippet>
    <content><![CDATA[
for (int i = 0; i < $1; ++i) {
    $0
}
]]></content>
    <tabTrigger>fori</tabTrigger>
    <scope>source.c++</scope>
    <description>Indexed For Loop</description>
</snippet>
```

### Long Long — `ll.sublime-snippet`

```xml
<snippet>
    <content><![CDATA[
long long
]]></content>
    <tabTrigger>ll</tabTrigger>
    <scope>source.c++</scope>
    <description>Long Long Type</description>
</snippet>
```

### Test Case Loop — `testcase.sublime-snippet`

```xml
<snippet>
    <content><![CDATA[
int t;
cin >> t;
while (t--) {
    solve();
}
]]></content>
    <tabTrigger>testcase</tabTrigger>
    <scope>source.c++</scope>
    <description>Multiple Test Cases</description>
</snippet>
```

To use any snippet, type its trigger and press `Tab`.

## 8. Locate Sublime Text User Settings

For reference, Sublime Text's Linux user data directory is generally:

```text
~/.config/sublime-text/Packages/User/
```

List its contents:

```bash
ls -la ~/.config/sublime-text/Packages/User/
```

You can also open the directory in the file manager:

```bash
xdg-open ~/.config/sublime-text/Packages/User/
```

The `User` directory is where your custom build system, snippets and personal preferences are stored.

## 9. Recommended Daily Workflow

1. Create a folder for a problem.
2. Open that folder in Sublime Text.
3. Create a `.cpp` file along with `input.txt` and `output.txt`.
4. Type `cp` and press `Tab` to insert the template.
5. Write and test your solution using the sample input.
6. Press `Ctrl+B` to compile and run.
7. Check the result in `output.txt`.
8. Copy the final solution to the online judge and submit it.

## 10. Troubleshooting

| Problem | What to check |
|---|---|
| `g++: command not found` | Install `build-essential`. |
| `Ctrl+B` uses the wrong compiler | Select `CP-C++17` from Tools → Build System. |
| `input.txt` not found | Create it beside the `.cpp` file. |
| `output.txt` is empty | Check compilation and execution errors in the build panel. |
| Snippet does not expand | Check the file extension, snippet scope and trigger. |
| Previous executable remains after a failed compile | Check the build panel; the old binary may still exist, but it will not run when compilation fails. |

## Official Resources

- [Sublime Text](https://www.sublimetext.com/)
- [Linux Repository Installation](https://www.sublimetext.com/docs/linux_repositories.html)
- [Sublime Text Build Systems](https://www.sublimetext.com/docs/build_systems.html)
- [Sublime Text Snippets](https://www.sublimetext.com/docs/snippets.html)
- [GCC Documentation](https://gcc.gnu.org/onlinedocs/)

---

**Repository:** `ubuntu-sublime-text-setup`

**Purpose:** Document a reproducible Sublime Text and C++17 workflow for Competitive Programming on Ubuntu.
