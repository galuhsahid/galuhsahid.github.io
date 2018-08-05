---
layout: post
title: "Notes on \"A Neural Algorithm of Artistic Style\""
tags: [paper]
featured-img: /images/posts/neural-artistic/las_meninas_velazquez.png
---

<blockquote><em>UPDATE:</em> I gave a talk on this topic at CS UI Dev Meetup #2, titled "Demistifying Neural Artistic Style Transfer". Slides are <a href="http://galuh.me/csuidevmeetup/slides">here!</a></blockquote>

I haven't read any paper in a while and I want to get into the habit of taking notes of them. So I thought I'd start with a paper that I actually really like. I love paintings (and art in general) so this makes it feel less like a chore (it doesn't feel like one at all!). Plus this reminds me of the Image Processing class that I took which I really enjoyed!

The paper that I'll be discussing today is ["A Neural Algorithm of Artistic Style"](https://arxiv.org/pdf/1508.06576) by Gatys et al. 

<figure class="figure">
    <img class="figure-image" src="/images/posts/neural-artistic/examples.png" alt="A picture of The Neckarfront in Tübingen 'painted' in the manner of various paintings.">
    <figcaption class="figcaption">
        <span class="figcaption__text">The Neckarfront in Tübingen 'painted' in the manner of various painting.</span>
    </figcaption>
</figure>

The main idea here is what makes fine art/artistic paintings what they are is "the interplay between the 'content' and the 'style' of the image". I instantly thought of the painting Las Meninas, which was originally painted by Velázquez but was recreated several times by Picasso. They did paint the same "content" (same furniture, same people, Infanta Margarita...), but each of the painting has a different style which makes each of them a unique piece and thus gives different impression to its observers.

<figure class="figure">
    <img class="figure-image" src="/images/posts/neural-artistic/las_meninas_velazquez.png" alt="A picture of the painting Las Meninas by Velázquez.">
    <figcaption class="figcaption">
        <span class="figcaption__text">Velázquez's Las Meninas.</span>
    </figcaption>
</figure>

<figure class="figure">
    <img class="figure-image" src="/images/posts/neural-artistic/las_meninas_picasso.png" alt="A picture of the painting Las Meninas by Picasso.">
    <figcaption class="figcaption">
        <span class="figcaption__text">Picasso's Las Meninas.</span>
    </figcaption>
</figure>

Up until the paper was written, there was no algorithm that could explain how it works or how to approach this problem. It all seems very abstract. The paper introduces the idea to derive artistic images by separating and recombining content and style of images using neural representations. 

## CNN

Convolutional Neural Networks (CNN) are the best deep neural networks for image processing. CNN are made up of units processing visual information hierarchically in a feed-forward manner. What do these layers do? We can think of each layer acting as an image filter, extracting a particular feature from the input image. 

OK, so how does this fit into the whole computer vision, object recognition fancy thingy? These CNN layers develop a recognition of the image that makes it easier for the computer to see the content of the image. Instead of seeing some random pixels smashed up together, after going through the CNN layers, our computer would actually recognize the content of the image thanks to the help of these layers.

## Where exactly is "content" in CNN?

In general, higher layers would capture high-level aspects of the content such as the objects themselves and how they are arranged (there's a sofa there! Next to it is a table!), but they do not give details on the exact pixel values. Because of this, we call the feature responses in the higher layers as the content representation. 

## Got it, now what about the "style"?
Right, now we know how "content" is represented in CNN. What about style? It's not as simple as getting the content representation. 

We use a feature space[^1] designed to capture texture information! Where is this feature space? Unlike the content representation, this feature space is present in every layer. What exactly does it contain? It contains correlations between the different features in all of the different layers of the CNN, and from this, we will get a multi-scale representation of the image that tells us about its texture information.

I did write that the feature space is present in every layer. Why does it have to be in every layer!? You may choose to define it locally by using only a few of the lower layers, if that tickles your fancy.  However, you'll end up with an image that looks like this:

<figure class="figure">
    <img class="figure-image" src="/images/posts/neural-artistic/style_subset.png" alt="The results when we define style only in a subset of the lower layers.">
    <figcaption class="figcaption">
        <span class="figcaption__text">The results when we define style only in a subset of the lower layers.</span>
    </figcaption>
</figure>

Compare this to the output image which feature space exists in every layer, giving a smoother, more visually appealing image:

<figure class="figure">
    <img class="figure-image" src="/images/posts/neural-artistic/style_all.png" alt="The results when we define style in all layers, including the higher layers.">
    <figcaption class="figcaption">
        <span class="figcaption__text">The results when we define style in all layers, including the higher layers.</span>
    </figcaption>
</figure>

## All right, anything else?
When we want to transfer the style of an existing art work to an input image, what we're actually doing is coming up with a variation of the image that:
- has the same content representation as the input image, and
- has the style representation of the artwork we want to transfer form.

That makes sense, right? Basically, we want a new image that has the same content as our photograph, but with the style of the artwork we want to "imitate". However, when combining an image with the style of an artwork there's a very, very small probability that your input image will match the constraints present in the artwork. This is we will use the loss function to determine how far/close is our generated image to the photograph's content and the artwork's style.

The interesting thing is that it turns out both the content and style representation are separable! These two things can be manipulated independently, as we can see later. Because both content and style are separable, we can choose to put more emphasize on one of the aspects. Focusing on content will yield an output image which can be easily identified by people. However, its style might be nowhere close to the artwork. Focusing on style will yield an output image which style is similar to the artwork and you can usually tell it. However, one may have trouble in determining what this image is all about (or rather, what is the content of the image).

## Methods
It's time for the nitty-gritty details!

- The CNN used in this paper is VGG-Network.
- 16 convolutional and 5 pooling layers of the 19 layer VGG-Network. No fully connected layer. 
- The max-pooling operation is replaced by average pooling, because it improves the gradient flow and yields more appealing results.

[VGG-Network](http://www.robots.ox.ac.uk/~vgg/research/very_deep/) is a network architecture developed by Oxford's  Visual Geometry Group (VGG), hence the name. It was placed first in localization task during the ImageNet ILSVRC-2014 and second in classification task. 

<figure class="figure">
    <img class="figure-image" src="/images/posts/neural-artistic/localization.png" alt="A picture that illustrates classification and localization, using kittens and puppies.">
    <figcaption class="figcaption">
        <span class="figcaption__text">Classification and localization. <a href="https://leonardoaraujosantos.gitbooks.io/artificial-inteligence">Source</a></span>.
    </figcaption>
</figure>

The details of the VGG-Network architecture can be seen [here](https://ethereon.github.io/netscope/#/gist/3785162f95cd2d5fee77).

I said earlier that both content and style are separable and we can work on them independently, so let's tackle them one by one! We'll first take a look at how to deal with the content first, then how to deal with the style, and finally we'll talk about how to tie everything together.

### Content representation
As you can see, there are a lot of layers. Which layer(s) play a part in reconstructing the "content"? These layers are `conv1_1`, `conv2_1`, `conv3_1`, `conv4_1`, and `conv5_1`. You can see in the network architecture that these are all the higher layers.

To extract the actual content of the input image (the photograph), we use gradient descent on a white noise image in hopes that the initial random image that we have (sometimes called as the white noise image) will evolve into the content of our original photograph. We use the higher layers, as mentioned above, because remember that higher layers capture the high-level aspects of the image! At this point, we have our "content loss" which is the squared-error loss between our photograph and our white noise image.

### Style representation

What about the "style"? I think we can say that it uses the same concept, with a few differences. Unlike the content representation, the style representation is present in every layer so we need a different treatment! What's actually in the layer is the correlations between the different filter responses, and these correlations are obtained from the Gram matrix (I'll get to this later). 

So I think this is like the key of the paper--that is, defining a) the separation between "content" and "style" and b) defining what a "style" is, because I think after reading the methods "content" seem to be more straightforward. So let's talk more about what these feature correlations are.

Here's the Gram matrix, which is the inner product between the vectorized feature map $$i$$ and $$j$$ in layer $$l$$:

$$G_{ij}^l = \sum_{k} F_{ik}^lF_{jk}^l$$

Ugh, equations. Just kidding.

So apparently the Gram matrix is able to filter out all the semantics/high-level aspects of the artwork and leave us with things such as textures, colors a.k.a. style! But I have a hard time understanding *why* the Gram matrix is capable of doing that, and I found a great explanation [here](https://medium.com/artists-and-machine-intelligence/neural-artistic-style-transfer-a-comprehensive-look-f54d8649c199). Let's take one more look at the equation. One last time I promise.

The Gram matrix, at least the one we need right now, is basically obtained from the multiplication of a matrix and its transpose:

$$G = V^TV$$

We're left with a matrix which entries consist of the multiplication of every row and column in the original matrix! Borrowing the term from the Medium post I linked before, the spatial information is "distributed". We've "lost" the high-level information such as what the objects are and where they are located, and instead we're left with "non-localized information"--information that is present in the artwork regardless where they are--which is the style! To be honest, this observation is mind-blowing to me, because it makes total sense from the artistic perspective! If we were to apply this on two different Monet paintings, say <em>Water Lily Pond</em> and <em>Springtime</em>, we'll end up with the same Monet textures (probably with different colors, but we can tell that these are definitely Monet's brushstrokes!!!) although they're both of different objects. I'M SO EXCITED.

From here, things work pretty much the way it works with content representation. We start with a white noise image and use gradient descent to come up with another image which style matches the style of the artwork. To measure how similar they are in style, we also measure the mean-squared distance between the entries in the Gram matrix of the artwork and the Gram matrix of the style we come up with so far. We can call this our "style loss". 

### Mix 'em up

All right! Up to this point we already have our generated image's content and style, as well as information on how far our generated image is from our photograph's content ("content loss") and artwork's style ("style loss").

How do we mix the content aspects and the style aspects, and determine that our generated image are close to both of these aspects? What we do is we minimize the distance between the white noise image (the generated image that we will continuously improve upon) and our original photograph (where we get our "content" from) AND the distance between the same white noise image and our artwork (where we get our style from). Intuitively, by minimizing both distances, we will end up with a generated image that is both closer to the content of our original photograph AND closer to the style of the artwork. This is called the total loss, which is obtained by adding the style loss and the content loss. We keep reducing the loss by backpropagating through the network, iteratively making our generated image closer to both our photograph (content-wise) and artwork (style-wise).

## And that's it!
If you're interested in the subject and the more rigorous mathematical details, I suggest you to read the paper! I found the paper to be very approachable and I found the organization of information to be really helpful. Many people have written their write-ups as well, so if you're confused about something and it's not answered in this post, I'm sure others have explained it (probably better than me!). 

I wrote this initially to gain more understanding of the paper itself, so I might skip some details to keep things concise (I try my best haha) and/or not go into details of things I think I'm already familiar with.

Let me know if you have any corrections! Peace out.

[^1]: A feature space is the n-dimensions where your features are located.