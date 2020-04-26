# WRITE-UP - PETITE FRAPPE 1

### SUBJECT

> Lors de l’investigation d’un poste GNU/Linux, vous analysez un fichier qui semble être généré par un programme d’enregistrement de frappes de clavier (enregistrement de l’activité de chaque touche utilisée).
> 
> Retrouvez ce qui a bien pu être écrit par l’utilisateur de ce poste à l’aide de ce fichier !
> 
> Format du flag : Pour valider ce challenge, vous devez insérer le contenu de ce qui a été écrit sur le clavier de ce poste dans la balise suivante `FCSC{xxxxxx}` puis soumettre votre réponse.

In this challenge, we have the extract of a keylogger, having recorded several keystrokes.  
Our goal is to find the written message (the flag in this case).  

### READING THE LOGS

In the given .txt file, this is what we have :

![keystrokes](/images/petite-frappe-1-keystrokes.png)

We can then quickly understand that the keyword `KEY_` corresponds to the touch that the user has pressed.  
The `value`, at `0` or at `1`, seem to mean "released", "pressed".  
So we can grep this keyword, and reconstruct the text.  

![keystroke-grep](/images/keystroke-grep.png)

The result is :  
`FCSC{unegentilleintroduction}`
