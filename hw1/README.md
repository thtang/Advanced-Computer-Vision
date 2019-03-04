<h1>Homework 1	<br> Image Matching (Detecting Motion Vectors) </h1>

<h3>Description</h3>

* Given two images:  trucka.bmp, truckb.bmp
* Detect motions vectors between trucka.bmp and truckb.bmp.
* Use trucka.bmp as the basis, sample it by an 8×8, 11×11, 15×15, 21×21, 31×31 block.

<h3>Algorithm:</h3>

* Block Matching Algorithm
* Sum of Absolute Difference is used as the cost function.
* Segment the image into blocks with a given size, e.g., 8x8. Redundant pixels are ignored. The location of top-left point for each block is recorded.
```python
def get_block_position(img, windowsize_r=8, windowsize_c=8, stride=8):
    patch_list = []
    position_tup = []
    # Crop out the window 
    for r in range(0,img.shape[0] - windowsize_r, stride):
        for c in range(0,img.shape[1] - windowsize_c, stride):
            position_tup.append((r,c))
            window = img[r:r+windowsize_r,c:c+windowsize_c]
            patch_list.append(window)
    return patch_list, position_tup
```
* Define a search range (50 pixels here) and find the block in basis image that can minize the cost function.
```python
def get_motion_and_position(patch_list_a, position_tup_a, patch_list_b, position_tup_b, search_range=50):
    motion_tup = []
    for patch_b, position_b in zip(patch_list_b, position_tup_b):
        cost = 999999
        for patch_a, position_a in zip(patch_list_a, position_tup_a):
            position_distance = ((position_a[0]-position_b[0])**2 + (position_a[1]-position_b[1])**2)**0.5
            if position_distance<=search_range:
                difference = np.sum(abs(patch_a-patch_b))
                if difference <= cost:
                    cost = difference
                    match_position = position_a
        dx, dy = match_position[0]-position_b[0], match_position[1] - position_b[1]
        motion_tup.append((position_b, match_position, (dx, dy)))
    return motion_tup
```
    
* The motion vector and top-left position for each block are recored for drawing plots.

<h3>Result</h3>

| block size  |  output |
|---|---|
|   8x8|<img src=https://github.com/thtang/Advanced-Computer-Vision/blob/master/hw1/output/block_8_motion_vector.png width=720>   |
|   11x11|<img src=https://github.com/thtang/Advanced-Computer-Vision/blob/master/hw1/output/block_11_motion_vector.png width=720>   |
|   15x15|<img src=https://github.com/thtang/Advanced-Computer-Vision/blob/master/hw1/output/block_15_motion_vector.png width=720>   |
|   21x21|<img src=https://github.com/thtang/Advanced-Computer-Vision/blob/master/hw1/output/block_21_motion_vector.png width=720>   |
|   31x31|<img src=https://github.com/thtang/Advanced-Computer-Vision/blob/master/hw1/output/block_31_motion_vector.png width=720>   |
