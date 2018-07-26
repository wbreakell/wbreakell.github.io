---
layout: post
title: "Hard and Symbolic Links"
---

Links in Unix allow us to reference one file from another in our filesystem. Conceptually, these links are very similar to the aliases one might see in the macOS Finder. Although both may appear similar, there are some fundamental differences in the way Unix links behave. As we'll see, these differences are entirely dependent on the types of links we choose to create.

In a Unix environment, there are two types of links we can create. The first type is known as a hard link, which can be created with a short command in our terminal.

{% highlight shell_session %}
$ ln linkedfile.txt hardlink
{% endhighlight %}

The `ln` command seen above takes two arguments and generates a hard link to a given source file in our filesystem. The first argument is the path to an existing file on our machine that we would like to link to. The second argument is the name of the new hard link we would like to generate. In the example above, we are creating a new hard link called `hardlink` which now references an existing source file named `linkedfile.txt`.

Hard links are unique in that they act as a direct reference to a file in our filesystem, providing us with another way of accessing the underlying data of that file. When we create a new hard link to a file, we are essentially just creating a new name for that file. Just as `linkedfile.txt` is a reference to a specific piece of data on our hard drive, `hardlink` is simply another name that references that same chunk of data. This means that if I move or delete the original source file, the hard link still works as expected because it's referencing the same underlaying data.

In our example above, we are essentially allowing two different names to reference the same piece of text content. If we make a change to the content of `linkedfile.txt` and then open `hardlink`, we will see that new, updated content. In fact, if we inspect both the original source file and the newly generated hard link, we will see that they are identical in size. Both files are referencing identical data.

{% highlight shell_session %}
$ ls -lahG
-rw-r--r--   2 walter  staff    23B Jul 25 22:58 hardlink
-rw-r--r--   2 walter  staff    23B Jul 25 22:58 linkedfile.txt
{% endhighlight %}

Hard links can be especially useful in a shared Unix environment, where we may want to create multiple references to a piece of data on our hard drive. When one user makes a change to that data, all of the users receive that change.

The second type of link in Unix is called a symbolic link. Symbolic links reference the path to a file or directory on our filesystem. These links don't keep track of any underlying data, they simply store the path to a file or directory on our system. This makes symbolic links far more brittle than hard links. If we move or delete the original source file, our symlink will break.

In order to create a symbolic link we will use the same `ln` command we used earlier, but with the addition of the `-s` option.

{% highlight shell_session %}
$ ln -s linkedfile.txt symlink
{% endhighlight %}

This command generates a new symbolic link named `symlink` that now references the path to `linkedfile.txt` in our filesystem. Let's take a closer look at this new symbolic link we just created.

{% highlight shell_session %}
$ ls -lahG
-rw-r--r--   2 walter  staff    23B Jul 25 22:58 linkedfile.txt
lrwxr-xr-x   1 walter  staff    14B Jul 25 23:00 symlink -> linkedfile.txt
{% endhighlight %}

Notice how the symbolic link visually points to the path it is referencing. This gives a great visual representation of what our symbolic link is actually keeping track of. In fact, if we look at the size of `symlink` we'll see that it is 14 bytes. This is the exact number of characters in our file path.

When trying to open or access a symbolic link, Unix simply reads the path to the referenced file or directory and performs the desired action as if we had typed out the entire path to the source file. This makes symbolic links useful when we just need to give ourselves a shortcut to another location on our system.

Both hard and symbolic links provide helpful ways of referencing different files across a Unix filesystem. Hard links provide a more direct link to a file, allowing one piece of data to be governed by two or more names. Symbolic links offer Unix users a more simplistic approach, allowing us to store and access the exact location of a file or directory on our machine.
