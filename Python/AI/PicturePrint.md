## plt.imshow()

接受`H*W*C`的`numpy.ndarray 或torch.Tensor` ,像素取值在0~1

```python
data = np.random.random((224,224,3))	# RGB

data.shape
plt.imshow(data)
```

```
(function) def imshow(
    X: ArrayLike,
    cmap: str | Colormap = ...,
    norm: Normalize = ...,
    aspect: float | Literal['equal', 'auto'] = ...,
    interpolation: str = ...,
    alpha: ArrayLike = ...,
    vmin: float = ...,
    vmax: float = ...,
    origin: Literal['upper', 'lower'] = ...,
    extent: Sequence[float] = ...,
    *,
    interpolation_stage: Literal['data', 'rgba'] = ...,
    filternorm: bool = ...,
    filterrad: float = ...,
    resample: bool = ...,
    url: str = ...,
    data: ... = ...,
    **kwargs: Any
) -> AxesImage
```



## cv2.imshow()

```python 
data = np.random.random((10,224,3))
data.shape
cv2.imshow('image',data)
if cv2.waitKey(-1) == 27:
    cv2.destroyAllWindows()
```







