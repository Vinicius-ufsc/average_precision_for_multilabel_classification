# <div align="center"> how to calculate average precision for a multilabel classification problem.</div>
##  <div align="center"> Step-by-step explanation with a simple example.</div>
<br>

> Let's consider a multilabel classification problem with 4 classes (A, B, C, D) and 5 samples (S1, S2, S3, S4, S5).<br>
The ground truth labels for each sample are given below:
<br>
<br>

## **Targets**

| Sample | A | B | C | D
|----------|-------|---------|-------|---------|
S1 | 1.00| 0.00| 1.00| 0.00|
S2 | 0.00| 1.00| 0.00| 0.00|
S3 | 1.00| 1.00| 1.00| 0.00|
S4 | 0.00| 0.00| 0.00| 1.00|
S5 | 1.00| 1.00| 0.00| 0.00|


> Now, let's assume that we have a classifier that predicts the following probabilities for each class and each sample:
<br>
<br>

## **Predictions**

| Sample | A | B | C | D
|----------|-------|---------|-------|---------|
S1 |0.80| 0.20| 0.65| 0.90|
S2 |0.30| 0.20| 0.40| 0.85|
S3 |0.20| 0.70| 0.45| 0.85|
S4 |0.10| 0.30| 0.70| 0.95|
S5 |0.70| 0.60| 0.45| 0.80|

>To calculate the mean average precision, we need to compute the precision-recall curve for each class, and then average the area under each curve.<br>

### Here are the steps to compute the precision-recall curve for class A:
<br>

## 1. Sort the samples by their predicted probability for class A in descending order:

```python
S1:  0.80  0.20  0.65  0.90  (A, C)
S5:  0.70  0.60  0.45  0.80  (A, B)
S2:  0.30  0.20  0.40  0.85  (B)
S3:  0.20  0.70  0.45  0.85  (A, B, C)
S4:  0.10  0.30  0.70  0.95  (D)
```

## 2. Compute precision and recall for each threshold:

$$
P = {TP \over TP + FP} \quad\quad\quad R = {TP \over TP + FN}  
$$

**Class A - Table:**

| Sample | probability | target
|----------|-------|---------|
| S1 | 0.80 | 1.00
| S5 | 0.70 | 1.00
| S3 | 0.20 | 1.00
| S2 | 0.30 | 0.00
| S4 | 0.10 | 0.00

<table>
<tr>
<th>Threshold = 0.80</th>
<th>Threshold = 0.70</th>
<th>Threshold = 0.30</th>
<th>Threshold = 0.20</th>
<th>Threshold = 0.10</th>
</tr>

<tr>
<td>

```python
TP = 1 (S1)
FP = 0
FN = 2 (S3, S5)
TN = 2 (S2, S4)
P = 1.00
R = 0.33
```
</td>

<td>

```python
TP = 2 (S1, S5)
FP = 0
FN = 1 (S3)
TN = 2 (S2, S4)
P = 1.00
R = 0.66
```
</td>

<td>

```python
TP = 2 (S1, S5)
FP = 1 (S2)
FN = 1 (S3)
TN = 1 (S4)
P = 0.66
R = 0.66
```
</td>

<td>

```python
TP = 3 (S1, S3, S5)
FP = 1 (S2)
FN = 0
TN = 1 (S4)
P = 0.75
R = 1.00
```
</td>

<td>

```python
TP = 3 (S1, S3, S5)
FP = 2 (S2, S4)
FN = 0
TN = 0
P = 0.60
R = 1.00
```
</td>

</tr>
</table>

## 3. Compute the area under the PR curve

* Join the points ( recall, max precision @ recall ) to create the curve.
>Possible recalls: 0.33, 0.66, 1.00<br>
>Max precisions:   1.00, 1.00, 0.75
<br>

<figura>


```python
AP_class_A = (1*0.66666) + (0.75*(1-0.66666)) = 0.9166575
```

## 4. Repeat for each class then average the result

<br>

### **Class B**


<table>
<tr>
<th>Threshold = 0.70</th>
<th>Threshold = 0.60</th>
<th>Threshold = 0.30</th>
<th>Threshold = 0.20</th>
</tr>

<tr>
<td>

```python
P = 1.00
R = 0.33
```
</td>

<td>

```python
P = 1.00
R = 0.66
```
</td>

<td>

```python
P = 0.66
R = 0.66
```
</td>

<td>

```python
P = 0.60
R = 1.00
```
</td>

</tr>
</table>

```python
AP_class_B = (1*0.6666) + (0.60)*(1-0.6666) = 0.86664
```

### **Class C**


<table>
<tr>
<th>Threshold = 0.70</th>
<th>Threshold = 0.65</th>
<th>Threshold = 0.45</th>
<th>Threshold = 0.40</th>
</tr>

<tr>
<td>

```python
P = undefined
R = 0.33
```
</td>

<td>

```python
P = 0.50
R = 0.50
```
</td>

<td>

```python
P = 0.50
R = 1.00
```
</td>

<td>

```python
P = 0.40
R = 1.00
```
</td>

</tr>
</table>

```python
AP_class_C = 1*0.50 = 0.50
```

### **Class D**


<table>
<tr>
<th>Threshold = 0.95</th>
<th>Threshold = 0.90</th>
<th>Threshold = 0.85</th>
<th>Threshold = 0.80</th>
</tr>

<tr>
<td>

```python
P = 1.00
R = 1.00
```
</td>

<td>

```python
P = 0.50
R = 1.00
```
</td>

<td>

```python
P = 0.25
R = 1.00
```
</td>

<td>

```python
P = 0.20
R = 1.00
```
</td>

</tr>
</table>

```python
AP_class_D = 1*1 = 1.00
```
<br>

>Finally we have:

```python
     A       B       C       D
AP = 0.9167, 0.8666, 0.5000, 1.0000
mAP = 0.8208
```

## Lets check with torchmetrics implementation of average precision.
<br>

```python
from torchmetrics.classification import MultilabelAveragePrecision

metric = MultilabelAveragePrecision(num_labels=4, average=None, thresholds=None)

pred = torch.tensor([[0.8, 0.20, 0.65, 0.90],
                     [0.3, 0.20, 0.40, 0.85],
                     [0.2, 0.70, 0.45, 0.85],
                     [0.1, 0.30, 0.70, 0.95],
                     [0.7, 0.60, 0.45, 0.80]])

targ = torch.tensor([[1., 0., 1., 0.],
                     [0., 1., 0., 0.],
                     [1., 1., 1., 0.],
                     [0., 0., 0., 1.],
                     [1., 1., 0., 0.]]).type(torch.int)

r = metric(pred, targ)
print(f'AP: {r}')
print(f'mAP: {torch.mean(r).item():.4f}')
```

```
out:
AP: tensor([0.9167, 0.8667, 0.5000, 1.0000])
mAP: 0.8208
```