# The Queen's Labyrinth

## The begining
lest start by saing that there is a binary, the binary is called maze.

when i read the binary i see that there is a check to return the flag

```
if ( v7 == 'E' )
      {
        puts(s: "\n[+] \"Curiouser and curiouser! Alice escaped the Queen's Labyrinth!\"");
        puts(s: "[+] The White Rabbit bows deeply and presents the secret flag:\n");
        __printf_chk(a1: 1, a2: "[+] Flag: ");
        v9 = enc_flag;
        do
          putc(c: *v9++ ^ 0x5A, stream: stdout);
        while ( v9 != (const char *)&unk_4023E8 );
        putc(c: 10, stream: stdout);
        return v8;
      }
```

so i need v7 to be 'E'
so i saw that there is a maze:
```
'###########'
'#S  # #   #'
'### # # # #'
'# # #   # #'
'# # # ### #'
'# # #   # #'
'# # ##### #'
'#   #   # #'
'# ### # # #'
'#     #  E#'
'###########'
```

i need to make the 'S' to be where the 'E' so i jast gave `DDSSSSSSAASSDDDDWWDDSSDD`

```     +---+---+---+---+   THE QUEEN'S LABYRINTH   +---+---+---+---+
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   "Off with her head!" shrieked the Queen of Hearts.
   Alice is lost among the thorny red rose hedges (#).
   Find the path to the Tea Party (E) before the cards catch you!
   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[?] Guide Alice through the maze (WASD): DDSSSSSSAASSDDDDWWDDSSDD

[+] "Curiouser and curiouser! Alice escaped the Queen's Labyrinth!"
[+] The White Rabbit bows deeply and presents the secret flag:

[+] Flag: BSidesTLV2026{qu33n_0f_h34rts_l4byr1nth}
```

## The end
