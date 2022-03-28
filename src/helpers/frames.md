# Frames

See `Appendix B` for more information.

## `TrainFrames`

Contains the following fields:
- `train`: the training set
- `validation`: the validation set
- `test`: the testing set

Use the `iid_without_test` class method to create a `TrainFrames` instance without the testing set.

## `CVFrames`

Contains a list of `TrainFrames`.

Use the `iid` class method to create an instance of `CVFrames`.
