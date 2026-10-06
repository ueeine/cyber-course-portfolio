step 1 get image from google

<img width="399" height="501" alt="cat" src="https://github.com/user-attachments/assets/abd2300f-2bab-45ab-b28c-36bfb8334eb5" />

## Step 2 Install the tools

Install these tools you will need

- steghide
- xxd 
- strings 
- exiftool 
- file
  
you can install them one by one or u can install them all in one command
```
sudo apt install steghide xxd binwalk libimage-exiftool-perl -y
```

 step 3 ceate secret msg
 ```
echo "secret msg" > secret.txt
cat secret.txt
```


<img width="337" height="112" alt="image" src="https://github.com/user-attachments/assets/c1d495d7-ac08-4c45-ad57-7dcb3c557a77" />


step 4 make copy of the original image
```
cp cat.jpg cat_copy.jpg
ls -l
```

<img width="645" height="247" alt="image" src="https://github.com/user-attachments/assets/f619699f-8b42-4bbb-a929-f12330ceb253" />


step 5 Hash both files before embedding
```
sha256sum cat.jpg cat_copy.jpg
```

<img width="720" height="97" alt="image" src="https://github.com/user-attachments/assets/8c820557-953d-49e4-a86b-d6a549621428" />


step 6 Embed the secret into cat.jpg 
```
steghide embed -cf cat.jpg -ef secret.txt
```


<img width="448" height="130" alt="image" src="https://github.com/user-attachments/assets/fc95478d-61e9-4a87-a8d9-b039fd9304b4" />


step 7 to view he binary of cat.jpg after embedding
```
xxd -b cat.jpg | head -30
```
<img width="681" height="644" alt="image" src="https://github.com/user-attachments/assets/a6c2f4fb-67f9-4143-8f49-80e40e3cb503" />


step 8 Hash both files again, after embedding 
```
sha256sum cat.jpg cat_copy.jpg
```

<img width="717" height="90" alt="image" src="https://github.com/user-attachments/assets/2eab6557-9b20-479a-8158-aa5808d5860c" />


Step 9 Delete the plaintext secret
```
rm secret.txt
ls
```

<img width="790" height="112" alt="image" src="https://github.com/user-attachments/assets/04e19d3b-c3e8-4d3f-9461-d52a4dea2ff8" />




step 10 Extract the hidden message from cat.jpg

```
steghide extract -sf cat.jpg
cat secret.txt
```

type the same passphrase password you used to embed. if you try to use wrong password it wont work you have to use the exact password u created

<img width="353" height="144" alt="image" src="https://github.com/user-attachments/assets/602a2f18-eee7-4702-a396-9f5ba65deb65" />

Step 11
Run detection tools
```
strings cat.jpg | grep -i secret
exiftool cat.jpg
binwalk cat.jpg
```
<img width="742" height="619" alt="image" src="https://github.com/user-attachments/assets/a037a162-8b7d-4d6e-8954-4028ca1f9321" />

next is task

file TechNova_Solutions.xlsx

<img width="428" height="65" alt="image" src="https://github.com/user-attachments/assets/342017f1-dea5-4fd2-b0b9-f38a12e9a169" />

i made cover audio file using espeak tool
<img width="1260" height="245" alt="image" src="https://github.com/user-attachments/assets/93bbf82a-6a22-47fc-b88d-6f79e750c6f2" />

then i embedded the spreadsheet
<img width="799" height="139" alt="image" src="https://github.com/user-attachments/assets/a33c7b71-5742-43fd-88cb-314b7c459095" />

i checked the embedded data to see if its there

<img width="668" height="129" alt="image" src="https://github.com/user-attachments/assets/ed93fb80-8059-4ea2-9c19-01594ee5a387" />

then i extracted the spreadsheet back
<img width="833" height="108" alt="image" src="https://github.com/user-attachments/assets/bd85d3a4-6eeb-4ba5-be6e-841c14b0273f" />

i checked the recovered file that it matches the original exactly
<img width="808" height="503" alt="image" src="https://github.com/user-attachments/assets/f36b6f73-ce96-4ff4-86e8-76fe2f9c2e5a" />
