## getline

* `getline(ss, line)` will not throw even if `ss` reaches end of file/string, in which case `line` will just be empty string.
* if no delimeter (`'\n', '\r'`) at the end of `ss`, `getline(ss, line)` will repeatedly return the final part of `ss` even after `ss.eof() == true`.

```
#include<iostream>
#include<string>
#include<sstream>

using namespace std;

int main()
{
  string s1 = "first line\n second line\n";
  // string s2 = "first line\n second line";
  stringstream ss{s1};
  // stringstream ss{s2};
  string line;
  for (int i = 0; i < 5; i++) {
    getline(ss, line);
    cout << ">" << line << "<" << endl;
    cout << ss.eof() << endl;
  }
}
```

# operator>>

```
stringstream ss;
string token;
ss >> token;
```

# istream::clear()

```
```