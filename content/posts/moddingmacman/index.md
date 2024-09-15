+++
title = "Reverse engineering .NET WPF Pacman using dnSpy"
date = "2024-09-15"
+++

**Reverse-engineering** - is the act of dismantling an object to see how it works. It is done primarily to analyze and gain knowledge about the way something works but often is used to duplicate or enhance the object.  


Watch on youtube - [https://www.youtube.com/watch?v=5Nqgh85c3JI](https://www.youtube.com/watch?v=5Nqgh85c3JI)

## Level - **easy**

In today's episode we are going to reverse engineer and modify an simple game written in C# .NET WPF.  

![pacman](pacman.png)

You are pacman (yellow guy), you can move in 4 directions (top, down, left, right), earn coins by eating them, winning by collecting 84 coins in total, however you can get eaten by one of the **ghosts**.  

It's a straightforward gameplay, but we're IT guys, which means we can bypass certain mechanics.  
And in this case we're going to:
1. Disable dying
2. Increse the amount of points out of the coins
3. Enable eating of ghosts
4. Enable flying through walls
5. Add text that we have modded it (leaving credit - important step haha)
6. Make main player faster
7. Make Ghosts edible, so we can earn some points of them too!

We will achieve this by using cool piece of software called [`dnSpy`](https://github.com/dnSpy/dnSpy) (discontinued but well made fork of ILSpy).  

# Stage 1 - Getting source code
**Compiling** is the process of converting human-readable code into machine-readable code. This is usually done by a software program called a compiler, which takes the source code and translates it into executable instructions for the computer to carry out.  

By doing **reverse engineering** we likely won't be able to get the initial code base, as after compiling some bits was replaced by something else, something was mooved, etc.  

But we still will be able to get most of the data that we need to amend changes.  

So let's dive into it, open up the `dnSpy` (x64/x32 depending on the application) and simply drag'n drop the `.exe` file of the game to the `dnSpy` and you should see something like this:  
![alt dnspy1](dnspy1.png)

Here you will need to find the Main class of the application, and from here you will be able to navigate through and find required place to do the changes.  
![alt text](structure.png)  
Here you can see all the game logic and variables, as it is very small.

You can click at each thing in this list and you should be able to see the code:  
![alt text](collisions.png)  
For example the collisions part.  
```c#
if ((string)rectangle.Tag == "wall")
{
    if (this.goLeft && this.pacmanHitBox.IntersectsWith(rect))
    {
        Canvas.SetLeft(this.pacman, Canvas.GetLeft(this.pacman) + 10.0);
        this.noLeft = true;
        this.goLeft = false;
    }
    if (this.goRight && this.pacmanHitBox.IntersectsWith(rect))
    {
        Canvas.SetLeft(this.pacman, Canvas.GetLeft(this.pacman) - 10.0);
        this.noRight = true;
        this.goRight = false;
    }
    if (this.goDown && this.pacmanHitBox.IntersectsWith(rect))
    {
        Canvas.SetTop(this.pacman, Canvas.GetTop(this.pacman) - 10.0);
        this.noDown = true;
        this.goDown = false;
    }
    if (this.goUp && this.pacmanHitBox.IntersectsWith(rect))
    {
        Canvas.SetTop(this.pacman, Canvas.GetTop(this.pacman) + 10.0);
        this.noUp = true;
        this.goUp = false;
    }
}
```  

# Stage 2 - Amending modifications  

Now we're going to disable collisions with the walls, let's simply delete the code block that you've seen before by Right-clicking at the code and then selecting `"Edit Method"`:  
![alt text](editcols.png)  
Now select code that you'd like to remove and press `"backspace"` button on your keyboard to delete it, click `"Compile"` if that's all changes you'd like to do to save changes.

## Changing score system 
Going further I've found the score incrementation logic:   
```c#
if ((string)rectangle.Tag == "coin" && this.pacmanHitBox.IntersectsWith(rect) && rectangle.Visibility == Visibility.Visible)
{
    rectangle.Visibility = Visibility.Hidden;
    this.score++;
}
```  
Let's fix it, and increase score by `50` each time when we *eat* the coin by changing logic to: 

```c#
this.score += 50;
```  
and that'll do.  

## Disable dying and gettin score out of ghosts  
```c#
if (this.pacmanHitBox.IntersectsWith(rect))
{
    this.GameOver("Well better luck next time!");
}
```  
Going to be:  
```c#
if (this.pacmanHitBox.IntersectsWith(rect))
{
    rectangle.Visibility = Visibility.Hidden;
	this.score += 200;
}
```  

And by doing that - our score will be incremented by 200 each time when we eat a ghost.  

## Leaving a credit  
```c#
this.txtScore.Content = "Score: " + this.score.ToString();
```  
->  
```c#
this.txtScore.Content = "Score: " + this.score.ToString() + "Mod by @denver-code";
```  

## Changing speeds  
Once you've found variables of the app:  
```c#
private int speed = 8;
private int ghostSpeed = 10;

```  
You can change them too:
```c#
private int speed = 15;
private int ghostSpeed = 5;
```

# Stage 3 - saving  
After you are happy will all the changes, smash that save button:   
![save](save.png)  
And export your cracked pacman game!  

![alt text](cracked.png)