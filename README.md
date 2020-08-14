# Catch-Err-All
 An universal command to quick check potential errors and interesting part in any source of log.

 ![Android](/sample_output1_android.png?raw=true "Sample output (Android)")
 ![Syslog](/sample_output2_syslog.png?raw=true "Sample output (syslog 1)")
 ![syslog2](/sample_output3_syslog.png?raw=true "Sample output (syslog 2)")


Tips:

[1] To show all lines, insert one | before closing ' , or use -A/B/C grep options to limit the lines.

[2] Use -v to view non-match lines, i.e. normal log.

[3] Patterns like "err" covers both "errno" and "interrupt", but I separate them to easier modify/recognize. You can remove them if you prefer shorter patterns.

[4] The negate style is in the format:

(?<!negate_prefix_1|negate_prefix_2)wanted(?!negate_postfix_1|negate_postfix_2)

, which the postfix don't have extra "<" unlike prefix.

[5] This command composes of 5 main sections divided by double \\, i.e. (?=\b|_) on right, (?<=\b|_) on left, (?<=\b|_) and (?=\b|_) on both side, \b on left, and nothing on both side. Each of them may divide into subsection by \, to easy to see prefix un/in/...etc—(many words) parts. Note that the patterns shouldn't strict to correct spelling, since log is written by anyone and need to allow incorrect spelling. And of course, nothing I can do if typo in log.

[6] The last two are "?" and "!" which normally indicate louder expression in log. But ! exclude <! html tag and `[ !` shell patterns to reduce noises, while still allow != because != quite often used to express something mismatch.

[7] Between '\ , next line \ , or ' , no extra space.

[8] Make 2 commands: `erra` (i.e. shows "a"ll lines) and `err` to easy to type:

    xb@dnxb:~/Downloads$ sudo ln -s /home/xiaobai/note/sh/Catch-Err-All/catch_err_all.run /usr/bin/err
    xb@dnxb:~/Downloads$ type -a erra # Make it alias or function in your shell startup script, such as ~/.bash_aliases
    erra is aliased to `err -holea'
    xb@dnxb:~/Downloads$ type -a err
    err is /usr/bin/err
    xb@dnxb:~/Downloads$ l /usr/bin/err
    8653011 lrwxrwxrwx 1 root root ? 53 Ogos 14 17:49 /usr/bin/err -> /home/xiaobai/note/sh/Catch-Err-All/catch_err_all.run*
    xb@dnxb:~/Downloads$

