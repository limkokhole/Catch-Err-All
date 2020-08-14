# Catch-Err-All
 A universal command to quick check potential errors and interesting part in any source of log.

 ![Android](/sample_output1_android.png?raw=true "Sample output (Android)")
 ![Syslog](/sample_output2_syslog.png?raw=true "Sample output (syslog 1)")
 ![syslog2](/sample_output3_syslog.png?raw=true "Sample output (syslog 2)")
 ![dmesg](/sample_output4_dmesg.png?raw=true "Sample output (dmesg)")
 ![gpu](/sample_output5_gpu.png?raw=true "Sample output (gpu)")


### Setup:

Make 2 commands: `erra` (i.e. shows "a"ll lines) and `err` to easy to type:

    # Make your path of this script executable (this is mine, change it to your path) :
    $ chmod +x /home/xiaobai/n/sh/Catch-Err-All/catch_err_all.run
    
    # Make a symlink in /usr/bin/err to yuor script path:
    $ sudo ln -s /home/xiaobai/note/sh/Catch-Err-All/catch_err_all.run /usr/bin/err 
    
    # Append alias or function in your shell startup script, such as ~/.bash_aliases , e.g.:
    $ echo -e 'function erra() {\n\terr -holea "$@"\n}\nexport erra' >> ~/.bash_aliases
    
    # Double check the setup :
    $ type -a erra
    erra is a function                                                                                                                                    
    erra ()                                                                                                                                               
    { 
        err -holea "$@"
    }
    $ type -a err
    err is /usr/bin/err
    $ ls -l /usr/bin/err
    lrwxrwxrwx 1 root root 53 Ogos 14 17:49 /usr/bin/err -> /home/xiaobai/note/sh/Catch-Err-All/catch_err_all.run

### How to use:

[1] Based on above setup, you can also manually type `-holea` option when using `err`, without require you edit left `err` to `erra`.

[2] This command is wrapper of `grep -niP`, you can add grep option as usual, such as `-r`.

[3] To show all lines, insert one | before closing ' , or use -A/B/C grep options to limit the lines.

[4] Use -v to view non-match lines, i.e. normal log.

### Customization:

[1] Patterns like "err" covers both "errno" and "interrupt", but I separate them to easier modify/recognize. You can remove them if you prefer shorter patterns.

[2] The negate style is in the format:

(?<!negate_prefix_1|negate_prefix_2)wanted(?!negate_postfix_1|negate_postfix_2)

, which the postfix don't have extra "<" unlike prefix.

[3] (?<=\b|_) (opening boundary) and (?=\b|_) (closing boundary) acts as word boundary \b except it allow underscore `_`

[4] This command composes of 5 main sections divided by double \\, i.e. (?=\b|_) on right, (?<=\b|_) on left, (?<=\b|_) and (?=\b|_) on both side, \b on left, and nothing on both side. Each of them may divide into subsection by \, to easy to see prefix un/in/...etc—(many words) parts. Note that the patterns shouldn't strict to correct spelling, since log is written by anyone and need to allow incorrect spelling. And of course, nothing I can do if typo in log.

[5] The last two are "?" and "!" which normally indicate louder expression in log. But ! exclude <! html tag and `[ !` shell patterns to reduce noises, while still allow != because != quite often used to express something mismatch.

[6] Between '\ , next line \ , or ' , no extra space. Keep in mind don't put double `|`, i.e. `||` manually in the script which causes shows all lines.

