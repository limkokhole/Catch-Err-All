# Catch-Err-All
A universal command for quickly checking potential errors and highlighting interesting parts in any log.

This tool mimics how humans read and identify error- or bug-related keywords in statements. It highlights these keywords, enabling users to spot them at a glance.

The selected keywords, which relate to errors, bugs, and other important or interesting elements, are based on my experience reading computer logs.

### Sample of use case:

 adb logcat:  
 ![Android](/images/sample_output1_android.png?raw=true "Sample output (Android)")

 syslog:  
 ![Syslog](/images/sample_output2_syslog.png?raw=true "Sample output (syslog 1)")
 ![syslog2](/images/sample_output3_syslog.png?raw=true "Sample output (syslog 2)")

  dmesg:  
 ![dmesg](/images/sample_output4_dmesg.png?raw=true "Sample output (dmesg)")

  gpu:  
 ![gpu](/images/sample_output5_gpu.png?raw=true "Sample output (gpu)")

  python:  
 ![python](/images/sample_output6_python.png?raw=true "Sample output (python)")

  apt:  
 ![apt](/images/sample_output7_apt.png?raw=true "Sample output (apt)")

  vpn:  
 ![ovpn](/images/sample_output8_ovpn.png?raw=true "Sample output (ovpn)")

 strace & wget:  
 ![wget](/images/sample_output9_strace_and_wget.png?raw=true "Sample output (strace & wget)")

### Setup:

Make 2 commands, `erra` (i.e. shows "a"ll lines) and `err`:

    $ git clone https://github.com/limkokhole/Catch-Err-All
    $ cd Catch-Err-All
    $ chmod +x erra
    $ chmod +x err
    $ sudo ln -s "$PWD"/err /usr/bin/err 
    $ sudo ln -s "$PWD"/erra /usr/bin/erra

### How to use:

[1] Similar to `grep`, i.e. `err /var/log/syslog`, `adb logcat | err`, `tail -f /var/log/syslog | err`  

[2] `erra` to show all lines, such as `dmesg | erra`, `adb logcat | erra`.  

[3] Based on above setup, you can also manually type `-all` option when using `err`, without require you edit left `err` to `erra`.  

[4] This command is wrapper of `grep --color=auto -niP`, you can add grep option as usual, such as `-r`.  

[5] In addition `erra`, use `-A`/`-B`/`-C` grep options with `err`(not `erra`) to limit the lines.  

[6] Use `err -v` to view non-match lines, i.e. normal log.  

[7] Since default already `-n`, you can use `-nn` (i.e. no line number) option on `err` or `erra` to hide line number.  

[8] Do `foo_command | grep --line-buffered bar | erra` OR `foo_command | unbuffer -p other_command bar | erra` if want to add extra grep or command.

### Customization:

[1] Patterns such as "err" covers "errno" and "interrupt" but I separate them to easier distinguish in a glance. 

[2] To avoid `internal PCRE error: 0` error, I need regex `()` as few as possible. You can check if your regex exceed default maximum by `echo malware | err`, it will produces `internal PCRE error: 0` error if regex too long. I noticed adding more entries in existing `(a|b|c|...)` and more `[-]?` doesn't causes this error. 

[3] The negate style is in the format:

`(?<!negate_prefix_1|negate_prefix_2)wanted(?!negate_postfix_1|negate_postfix_2)`

, which the postfix don't have extra `<` unlike prefix.

This format useful to avoid treats common tag `DEBUG` as important keyword `bug`.

[4] `(?<=\b|_)` (opening boundary) and `(?=\b|_)` (closing boundary) acts as word boundary `\b` except it allow underscore `_`

[5] This command composes of 6 main sections divided by double `\\`(2 continuous lines of `\`), i.e. `(?=\b|_)` on right, `(?<=\b|_)` on left, `(?<=\b|_)` and `(?=\b|_)` on both side, `\b\w*` on both side, `\b` on both side, and nothing on both side. Each of them may divide into subsection by `\`, to easy to see prefix un/in/...etc—(many words) parts. Note that the patterns shouldn't strict to correct spelling, since log is written by anyone and need to allow incorrect spelling. And of course, nothing I can do if typo in log.

[6] The last two are "?" and "!" which normally indicate louder expression in log. But ! exclude <! html tag and `[ !` shell patterns to reduce noises, while still allow != because != quite often used to express something mismatch.

[7] Between `'\` , next line `\` , or `'`, no extra space. Keep in mind don't put double `|`, i.e. `||` manually in the script which causes shows all lines.

[8] You can customize matched color of `grep` with e.g. `export GREP_COLORS='ms=01;33'`.

