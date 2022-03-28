# Publishing your model

Once you are done implementing the model, and all unit tests have passed, it is time to publish your work.

Before you do it, make sure you have access to [MoBagel's docker harbor](harbor.mobagel.com/exodus/).

Then run the command:
```bash
poe save
```

This builds the model algorithm into a docker image, pushes that image to MoBagel's Docker harbor, then saves it to a tarball.

To incorporate it into the Exodus structure, simply copy the tarball to the main repo's `images/` directory, or set the main repo's configuration so that it pulls images from the MoBagel harbor.
