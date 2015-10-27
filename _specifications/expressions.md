---
layout: specification
description: Expression DSL
---
The Expression meta-model is used to specify the expressions defined in an Ecore meta-model. It is used in EEF in order to decorate the EEF meta-model expressions.

##### User stories

###### US-1 As an EEF contributor, Claire wants to decorate the expressions defined by the EEF meta-model
Claire needs to provide an *Expression* in her EEF meta-model. An *expression* will be interpreted using the available interpreters in the EEF runtime. In order to provide details about the expressions, she uses the Expression meta-model to decorate her EEF meta-model.

As the root concept of our Expression meta-model, we have the concept of *SiriusExpressionPackage*. A *SiriusExpressionPackage* has an *ePackage* in order to reference an existing meta-model ePackage. Claire defines the *ePackage* "eef" to reference her EEF meta-model.

New concepts:

* SiriusExpressionPackage
* SiriusExpressionPackage#ePackage: EPackage [0..1]

An Xtext based editor is available to edit "xxx.expression" files.
{% highlight java linenos %}
package eef{
}
{% endhighlight %}

###### US-2 As an EEF contributor, Claire wants to decorate an expression defined by an eClass of the EEF meta-model
Claire needs to decorate an expression defined by an eClass of the EEF meta-model. She specifies a new *SiriusExpressionClass* which is contained by the *SiriusExpressionPackage* reference *expressionClasses*. In order to reference the EEF meta-model eClass she wants to decorate, she defines the *eClass* feature to reference the "EEFViewDescription" eClass.

New concepts:

* SiriusExpressionClass
* SiriusExpressionPackage#expressionClasses: SiriusExpressionClass [0..-1] containment
* SiriusExpressionClass#eClass: EClass [0..1]

{% highlight java linenos %}
package eef{
	class eef.EEFViewDescription{	
	}
}
{% endhighlight %}


###### US-3 As an EEF contributor, Claire wants to define a new variable for her expression
Claire wants to define a new variable which can be used by her expression. She provides a *SiriusVariable* which is contained by the expression class reference *variables*. A *SiriusVariable* has a *name* to identify it, a *documentation* to provide a description and an *eType* to define the type of the associated element. In our EEF example, Claire defines a variable named *viewSemanticCandidate* which represents the semantic element associated to the properties view. This variable can be used then in the expression: "aql:viewSemanticCandidate.name".

New concepts:

* SiriusVariable
* SiriusExpressionClass#variables: SiriusVariable [0..-1] containment
* SiriusVariable#name: EString [0..1]
* SiriusVariable#documentation: EString [0..1]
* SiriusVariable#eType: EClassifier [0..1]

{% highlight java linenos %}
package eef{
	class eef.EEFViewDescription{
		var viewSemanticCandidate : ecore.EObject
	}
}
{% endhighlight %}

###### US-4 As an EEF contributor, Claire wants to describe the return type and the bounds of an expression
Claire needs to detail the labelExpression attribute defined by the EEFViewDescription eClass. She specifies a new *SiriusExpressionDescription* which is contained by the *SiriusExpressionClass* reference *expressionDescriptions*. She references the "labelExpression attribute" defined in the EEF meta-model by using the *expression* attribute of the Expression meta-model. She specifies the expression's *returnType* to "EString" and she specifies the cardinality of the expression result with the *lowerBound* and *upperBound* attribute. She sets them respectively to "0" and "1".

New concepts:

* SiriusExpressionClass#expressionDescriptions: SiriusExpressionDescription [0..-1] containment
* SiriusExpressionDescription#expression: EStructuralFeature [0..1]
* SiriusExpressionDescription#lowerBound: EInt [0..1]
* SiriusExpressionDescription#upperBound: EInt [0..1]
* SiriusExpressionDescription#returnType: EClassifier [0..1]

{% highlight java linenos %}
package eef{
	class eef.EEFViewDescription{
		var viewSemanticCandidate : ecore.EObject
		exp eef.EEFViewDescription.labelExpression() : ecore.EString [0..1]{
		}
	}
}
{% endhighlight %}


###### US-5 As an EEF contributor, Claire wants to define the variables which can be used by her expression.
Claire is able to define variables. She wants to reuse these variables in several expressions. This is possible with the *SiriusParameter* concept. The parameter references an existing *SiriusVariable*. She can provide a *SiriusParameter* contained by the expression reference *parameters*. A *Parameter* can be *optional*, this means that the variable may not be set. The SiriusVariable can be considered as static variables usable from any expression.

New concepts:

* SiriusParameter
* SiriusExpressionDescription#parameters: SiriusParameter [0..-1] containment
* SiriusParameter#optional: EBoolean [0..1]
* SiriusParameter#variable: SiriusVariable [0..1]

{% highlight java linenos %}
package eef{
	class eef.EEFViewDescription{
		var viewSemanticCandidate : ecore.EObject
		exp eef.EEFViewDescription.labelExpression(viewSemanticCandidate) : ecore.EString [0..1]{
		}
	}
}
{% endhighlight %}

###### US-6 As an EEF contributor, Claire wants to specify the elements on which the EEF specifier can provide variables
Claire wants to define all the variables that are accessible by her expression. It exists two kind of variables:
* the ones that are defined by Claire when she defined the expressions in the EEF meta-model. We already present these variables as *SiriusVariables*.
* the others are defined by Zoe when she instanciates the EEF meta-model to specify her properties view. These variables are defined dynamically at runtime. In order that the EEF runtime will be able to find these dynamic variables, Claire needs to define which elements can define dynamic variable for an expression. This is done by referencing EEF classes with the *userDefinedVariableContainers* reference. In the following example, Zoe can specifies *UserDefinedVariables* on an EEFWidgetDescription and then she can use these variables in the labelExpression.

New concepts:

* SiriusExpressionDescription#userDefinedVariableContainers: EClass [0..-1]

{% highlight java linenos %}
package eef{
	class eef.EEFWidgetDescription {
		exp eef.EEFWidgetDescription.labelExpression(viewSemanticCandidate, pageSemanticCandidate, groupSemanticCandidate, containerSemanticCandidate) : ecore.EString [0..1] {
			userDefinedVariableContainers = [
				eef.EEFViewDescription, eef.EEFPageDescription, eef.EEFGroupDescription, eef.EEFContainerDescription, eef.EEFWidgetDescription
			]
		} 
	}
}
{% endhighlight %}

![Expression meta-model usage]({{ site.baseurl }}specifications/images/expression-metamodel-usage "Expression meta-model usage") 

###### US-7 As an EEF contributor, Claire wants to specify that an expression returns nothing 
Claire wants to define an expression which does not return any element. For example, Claire specifies the onClickExpression which represents the action called after the final user clicked on a link in the properties view. In this case she defines as returnType for the expression: "Void".

New concepts:

* Void

{% highlight java linenos %}
package eef{
	class eef.EEFLinkDescription {
		exp eef.EEFLinkDescription.onClickExpression(viewSemanticCandidate) : expression.Void [0..1] {
		} 
	}
}
{% endhighlight %}

###### US-8 As an EEF contributor, Claire wants to specify that an expression returns an object 
Claire wants to define an expression which returns an object. For example, Claire specifies the valueExpression of an EEFImageDescription which represents an image showed in the properties view. In this case she defines as returnType for the expression: "Object".

New concepts:

* Object

{% highlight java linenos %}
package eef{
	class eef.EEFImageDescription {
		exp eef.EEFImageDescription.valueExpression(viewSemanticCandidate) : expression.Object [0..1] {
		}
	}		
}
{% endhighlight %}

###### US-9 As an EEF contributor, Claire wants to specify that an expression returns a predicate 
Claire wants to define an expression which returns a predicate. For example, Claire specifies the selectablePredicateExpression of an EEFInterpretedTreeStructureDescription which represents the predicate which must be matched by eleemnts that could be selectable in a tree in the properties view. In this case she defines as returnType for the expression: "Predicate".

New concepts:

* Predicate

{% highlight java linenos %}
package eef{
	class eef.EEFInterpretedTreeStructureDescription {
		exp eef.EEFInterpretedTreeStructureDescription.selectablePredicateExpression(viewSemanticCandidate) : expression.Predicate [0..1] {
		} 
	}		
}
{% endhighlight %}