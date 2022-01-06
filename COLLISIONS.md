# Collisions

## 1. Ground collisions 

### 1.1. The issue

So, at first I thought it would be pretty easy:

Imagine (in a 3d scene) there's a player with id `P`, and ground with id `G`.
I thought that just checking if `P.y <= G.y` would detect if player is touching `G`... but I realized there was a simple issue: `P.y` was the `y` cordinate of the CENTER of the player:

```
 O
 | -> P.y is here
 |
/ \
```

So I had 2 options:

1. Make the player `y` coord be in it's lowest point
2. Change the way it detect's the ground collision

### 1.2. Simple math goes brrrrrrrrrrrrrrrrr

And at first I just said "Well, let's move the player y way down". But in that case, it wouldn't work if I use the same for a ceiling collision.

And I went with option 2. My solution was a simple math operation (yes, more math...):

```cpp
int playerLowestPoint = P.y - (P.h / 2);
```

Let me explain. If the player `y` coord is always the center of it's height (`P.h / 2`),
we can remove it to the current `y` position and get the `y` coord of the player minus
the half of it's height (which is the position of it's `y` coord).

Imagine `P.y` is 200 and the player height's 50.

The player `y` coord is get from half of the player's height (`50 / 2` = `25`).

So it's lowest point will be `P.y - (P.h / 2) = 200 - (50 / 2) = 200 - 25 = 175`.
And if `G.y` is `175`, it will detect a collision and it will stop falling.


## 2. Why what I said doesn't really work

Imagine you have this

```
 O
 | -> P.y is here
 |
/ \

------------------
   G R O U N D
------------------

        -> empty space

-----------------------
 M O R E  G R O U N D
-----------------------
```

If the player wants to enter the empty space and we're detecting the upper ground, the player can't really enter because it's lower than the ground, and it detects collision.

## 3. Maybe now? Can you just say what would work?

<https://developer.mozilla.org/en-US/docs/Games/Techniques/3D_collision_detection#point_vs._aabb>

Yes, all this for just a link, I'm very tired

