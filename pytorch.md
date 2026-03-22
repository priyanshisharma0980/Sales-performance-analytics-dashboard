Deep learning librabry used to make neural networks   
Neural network- a computational model inspired by the human brain's structure, consisting of interconnected nodes ("neurons") that learn patterns from data   
to perform complex tasks like pattern recognition and prediction.   

Deep learning is a specialized subset of machine learning and artificial intelligence (AI) that uses multi-layered artificial neural networks     
to mimic the human brain's learning process.    

Core features-   
using python compatability    
dynamic computational graph - represnt mathematical calculations with graph    
Tensor computauion (multidimension data)   

Offer TorchScript for model serialization   

CORE MODULES    
Torch   
torch.autograd
torch.nn   
torch.utli.data   
torch.onnx   


### TENSORS
Multidimensional array designed for mathematical and computational efficency    
Dimensions-

Scalar - a single number, 0 dimension    
Vectors - 1 dimesnion tensor , array     
Metrices - 2 dimension like a grid    
3d tensors- Coloured image RGB IMAGE   
4d tensors - multiple rgb images   
5d tensor - video data, it has batches and frames (combination of images)   


### create a tensor  

on google collab as gives facility for GPU   
import torch   
print(torch.__version__) 2.5.1    

a= torch.empty(2,3) will create an empty tensor of size (2,3)    
type(a)    

torch.zeros(2,3)    
torch.rand(2,3)    

torch.tensor([1,2,3],[4,5,6])   
torch.linspace(0,10,10)   
torch.arange(0,10,2)   


x= torch.tensor([1,2,3],[4,5,6])    
x.shape   

### check data type
x.dtype   
x= torch.tensor([1,2,3],[4,5,6], dtype = torch.int32)   

### MATHEMATICAL opertaions on tensor 
x= torch.rand(2,2)  
x   

x+2, x-2, x*2, x/2   

Between 2 tensors of same shape   
x+b, x-b

Other-       
torch.abs(x)   
abs (absolute) - turns all values to positive    
neg  - turns into negatove value   
round, ceil (rounds off to upper integer 2.3=3), floor    

torch.mean, torch.sum, torch.transpose, flatten, reshape      

Eg- when we perform opertaions between 2 tensors what happens is the output will be 3rd tensor and takes place in memory   
m+n this will create a new tensor  
to avoid this m.add_(n), it shtores the result in m   

COPY a tensor    
a=b (we can use this, but once a is chnaged b also changes)   
so we use b = a.clone()   it iwll copy data from a to b in a differnt memory location   

id(a) - gives the location in memory   


### check avalability of GPU 
torch.cuda.is_available() it will give True   
device = torch.device('cuda')    

CPU has RAM, GPU has VRAM   GPU is faster   

### convert tensor to numpy
b = a.numpy()
numpy to tensor  - 
c= torch.from_numpy(b)    
 




























