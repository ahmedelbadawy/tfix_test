## TFix: Learning to Fix Coding Errors with a Text-to-Text Transformer


TFix is a state-of-the-art system for automatically fixing coding errors in programs. The key idea behind TFix is to leverage a large text-to-text Transformer pre-trained on natural languages. This design allows TFix to apply a knowledge transfer between natural and programming languages. In addition to that, TFix is fine-tuned jointly on 52 different error types, which allows it to learn typical patterns across various error types together.



We have loaded T5 base model which is already trained a large-scale dataset of ∼100k aligned pairs of coding errors and fixes from 5.5 million GitHub commits, then we have predicted the output for some test examples of the different error types, and we generated jason file which have
* source code: is the code which have an error
* target code: is the code after being corrected by a human developer
* prediction: is the corrected code predicter by the model
* correct: It's true if the code suggested by the model matches the target code

The model has accuracy of 0.49 which is matching with the target source without, this percentage will increase when using error removal as a metric also as it counts a prediction as correct if the error is removed, in the paper it reached accuracy of 0.67

You will find the test results in test_data.json file and accuracy in each error type in first_acc.txt file in the folder [t5model_results](t5model_results)

### Here are some examples from the results
```
{
      "source_code": "  if (element === undefined) {\n    throw 'getEnabledElement: parameter element must not be undefined';\n  }\n",
      "target_code": "  if (element === undefined) {\n    throw new Error('getEnabledElement: parameter element must not be undefined');\n  }\n",
      "predection": "if (element === undefined) { throw new Error('getEnabledElement: parameter element must not be undefined'); }",
      "error id": "no-throw-literal",
      "correct": "True"
}


{
      "source_code": "\n  var self = this\n  self.client = WebDriver.remote(options)\n",
      "target_code": "\n  self.client = WebDriver.remote(options)\n",
      "predection": "self.client = WebDriver.remote(options)",
      "error id": "no-redeclare",
      "correct": "True"
}

{
      "source_code": "const dbDump = function(env, done) {\n  path = env !== 'prod' ? 'dev' : env;\n  const date = moment();\n",
      "target_code": "const dbDump = function(env, done) {\n  const envPath = env !== 'prod' ? 'dev' : env;\n  const date = moment();\n",
      "predection": "const dbDump = function(env, done) { let path = env!== 'prod'? 'dev' : env; const date = moment();",
      "error id": "no-const-assign",
      "correct": "False"
},
```

