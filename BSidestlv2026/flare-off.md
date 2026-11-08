lest start by saing that there is a binary, the binary is called flare-off.
the binary ask for an input, each time i give a input it gives me a flag
i saw in the code that there is a function to check the flag. it called 'check_master_key'
the function do a xor using a string = "hum4n_1nt3ll1g3nc3_tr1umph5_0v3r_5t0ch45t1c_p4rr0t5". it checks if the flag ^ string = 0, so the flag is the string right. no, but when i give the string it return:
"flare-off{0k_th15_15_th3_r34l_fl4g_ju5t_h45h_th3_b1n4ry_w1th_5h4256_4nd_put_1t_1n_th3_u5u4l_fl4g_f0rm4t_t0_w1n}"
this is also not the flag, but when reading what it says i need to sha256 it and then put in inside "BSidesTLV2026{}"
if i sha256 the flag i get "060902f75ba2cff4e44c5731b975edba4122e5655acfc4dc66a8f004ab000885"
so "BSidesTLV2026{060902f75ba2cff4e44c5731b975edba4122e5655acfc4dc66a8f004ab000885}"
