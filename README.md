# CCC-challenge

CCC (or Content Creators Club) was a CTF run by Cyber Discovery which I took part in and created a challenge for. This is the repo for my challenge (cm03), which I guess would be in the misc category- involving some OSINT and basic stego/ crypto methods. 

# Title - DESmond's peculiar image

## Briefing

We've had our eyes on someone called Desmond Ivanovich. From a first look, he seems like rather not much more than a book nerd, but we think he may be using his platforms as a front to send secret messages out to the public. His twitter handle can be found as @des_ivanovich - see if you can dig up anything else. 

### By das

# Infrastructure

I don't believe my challenge needs any infrastucture, as the only file that needs to be downloaded is from a reddit page. 

# Risks

I also don't believe there are any risks to my challenge.

# Walkthrough

The participant is given the twitter handle ```@des_ivanovich``` (https://twitter.com/des_ivanovich). Navigating to this account, I have filled it with a few unrelated tweets so that its not terribly easy and the osint lives up to its name. 

------

First, I shall explain my _'rickroll'_. I'm assuming you know what this is, the infamous redirect to Rick Astley's 'Never going to give you up'- a common occurance in CTFs. 

Looking at the pinned tweet on des's profile, we see the link to his GitHub. As you can see, I have tried to steer them in the wrong direction. Once clicked on the link, they are met with des' GitHub profile, with one repository- `MaybeAFlag`.

This is where the wild goose chase starts. 

Inside the folder is another folder, and inside this another, and so on. Each with small message encouraging them to go on. This goes on for a while, till the last three include a fake flag of ```cccctf{always_gonna_give_you_up}``` chunked across the last 4 folders. 

In the last folder, FlagFile, is a flag.zip. I wrote an encouraging message to download and listen to the mp3 inside it. 

Here is a screenshot of the folders (and contents of the final folder) for reference:

![image](https://user-images.githubusercontent.com/66535878/84587822-60860500-ae1a-11ea-8fa5-e12514192088.png)

You guessed it- inside is the entirety of 'Never Gonna Give You Up', delivered in mp3 format. 


------

After realising the rabbit hole they went down, hopefully the particpant will venture back to the twitter page and dig deeper. 

Looking further into the tweets, there is an image of the cover of Crime and Punishment by Fyordor Dostoyevsy which is my stego file. I had some issues with uploading the stego file straight onto twitter since it deleted the additional messages I added in it, so instead I replied to the tweet with a link to a reddit page, and a couple cryptic messages to urge participants on the right track. 

Following this link found in the replies: https://www.reddit.com/user/des_ivanovich,
we see the same image, however this time there are a couple of hidden messages inside. 

Downloading the file then running `strings` on it (alternatively opening it in a text editor) we see: 

`110 141 150 141 41 40 131 157 165 40 146 157 165 156 144 40 155 145 40 100 156 145 166 145 162 137 142 145 137 146 60 165 156 144`

`QnV0IG1heWJlIHRoZXJlIGlzIG1vcmUgdG8gYmUgZm91bmQgaW4gdGhpcyBpbWFnZS4uLg==`

at the bottom. 

Simply encoded, the former string decodes `from octal` to get-  `Haha! You found me @never_be_f0und`
and the latter an obvious `from base64` to get- `But maybe there is more to be found in this image...`

Now we have what seems to be another twitter handle, however the second string indicates we need to do something else. 
As with the classic stego challenge, running the command `steghide --extract -sf CP.jpg` will give us the next part. 
`steghide` will extract as so:

```bash
kali@kali:~/Downloads$ steghide --extract -sf CP.jpg
Enter passphrase: 
wrote extracted data to "ooooooooo.txt".
kali@kali:~/Downloads$ cat ooooooooo.txt 
A: "I can unlock many doors."
B: "well... I initialise the race"
A: "I am the ___ to your success"
B: "Okay we get it. At least i'm not scalar."


A = D3hs12o8cI9lzar58f1337
B = L33Tv3cT0rR1gHtH3r300cccctf

kali@kali:~/Downloads$
```

Hopefully from my cryptic message one may figure out that the last step of this challenge is cryptography of some sort. Seeing as A is a key, and B is an iv, the options have somewhat been narrowed. 

Not forgetting the second twitter handle we are given (`never_be_f0und`), once we navigate here in the bio we see this string:

`hAp1cUrE_z3:03n9n70172or19p827733q82qr6rs316o008ors8pnr098q2pps49737n09qnsqn`

Since no one can ever get quite enough of rot13, I added it in. Decipher (in cyberchef tick both shift upper case and lower case characters) to get:

`uNc1pHeR_m3:03a9a70172be19c827733d82de6ef316b008bef8cae098d2ccf49737a09dafda`

Now that one has their key, iv and ciphertext, to cyberchef we go.
Noticing the title (DESmond), perhaps it may be DES decrypt?

Put it into DES decrypt- mode CBC- and wahey, we have our flag:


![image](https://user-images.githubusercontent.com/66535878/84575262-bf5c6780-ada3-11ea-98f1-0107e399d344.png)


# Flag

```cccctf{sup3r_cTf_sK1llz_:D}```


*note- following a test run with someone, I was told that passing the key and iv as 'hex' even though they are not is slightly confusing, therefore I have since added a note to the bottom of `ooooooooo.txt` as follows:

`please note that A and B are to be regarded as 'hex' values in your decryption. Happy decoding!`



(apologies if any of my grammar in russian is off)






