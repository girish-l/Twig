Deprecated Features
===================

This document lists deprecated features in Twig 3.x. Deprecated features are
kept for backward compatibility and removed in the next major release (a
feature that was deprecated in Twig 3.x is removed in Twig 4.0).

Functions
---------

 * The ``twig_test_iterable`` function is deprecated; use the native PHP
   ``is_iterable`` function instead.

Extensions
----------

* All functions defined in Twig extensions are marked as internal as of Twig
  3.9.0, and will be removed in Twig 4.0. They have been replaced by internal
  methods on their respective extension classes.

  If you were using the ``twig_escape_filter()`` function in your code, use
  ``$env->getRuntime(EscaperRuntime::class)->escape()`` instead.

* The following methods from ``Twig\Extension\EscaperExtension`` are
  deprecated: ``setEscaper()``, ``getEscapers()``, ``setSafeClasses``,
  ``addSafeClasses()``. Use the same methods on the
  ``Twig\Runtime\EscaperRuntime`` class instead:
  
  Before:
  ``$twig->getExtension(EscaperExtension::class)->METHOD();``
  
  After:
  ``$twig->getRuntime(EscaperRuntime::class)->METHOD();``

Nodes
-----

* The second argument of the
  ``Twig\Node\Expression\CallExpression::compileArguments()`` method is
  deprecated.

* The ``Twig\Node\Expression\NameExpression::isSimple()`` and
  ``Twig\Node\Expression\NameExpression::isSpecial()`` methods are deprecated as 
  of Twig 3.11 and will be removed in Twig 4.0.

* The ``filter`` node of ``Twig\Node\Expression\FilterExpression`` is
  deprecated as of Twig 3.12 and will be removed in 4.0. Use the ``filter``
  attribute instead to get the filter:

  Before:
  ``$node->getNode('filter')->getAttribute('value')``

  After:
  ``$node->getAttribute('twig_callable')->getName()``

* Passing a name to ``Twig\Node\Expression\FunctionExpression``,
  ``Twig\Node\Expression\FilterExpression``, and
  ``Twig\Node\Expression\TestExpression`` is deprecated as of Twig 3.12.
  As of Twig 4.0, you need to pass a ``TwigFunction``, ``TwigFilter``, or
  ``TestFilter`` instead.

  Let's take a ``FunctionExpression`` as an example.

  If you have a node that extends ``FunctionExpression`` and if you don't
  override the constructor, you don't need to do anything. But if you override
  the constructor, then you need to change the type hint of the name and mark
  the constructor with the ``Twig\Attribute\FirstClassTwigCallableReady`` attribute.

  Before::

      class NotReadyFunctionExpression extends FunctionExpression
      {
          public function __construct(string $function, Node $arguments, int $lineno)
          {
              parent::__construct($function, $arguments, $lineno);
          }
      }

      class NotReadyFilterExpression extends FilterExpression
      {
          public function __construct(Node $node, ConstantExpression $filter, Node $arguments, int $lineno, ?string $tag = null)
          {
              parent::__construct($node, $filter, $arguments, $lineno, $tag);
          }
      }

      class NotReadyTestExpression extends TestExpression
      {
          public function __construct(Node $node, string $test, ?Node $arguments, int $lineno)
          {
              parent::__construct($node, $test, $arguments, $lineno);
          }
      }

  After::

      class ReadyFunctionExpression extends FunctionExpression
      {
          #[FirstClassTwigCallableReady]
          public function __construct(TwigFunction|string $function, Node $arguments, int $lineno)
          {
              parent::__construct($function, $arguments, $lineno);
          }
      }

      class ReadyFilterExpression extends FilterExpression
      {
          #[FirstClassTwigCallableReady]
          public function __construct(Node $node, TwigFilter|ConstantExpression $filter, Node $arguments, int $lineno, ?string $tag = null)
          {
              parent::__construct($node, $filter, $arguments, $lineno, $tag);
          }
      }

      class ReadyTestExpression extends TestExpression
      {
          #[FirstClassTwigCallableReady]
          public function __construct(Node $node, TwigTest|string $test, ?Node $arguments, int $lineno)
          {
              parent::__construct($node, $test, $arguments, $lineno);
          }
      }

* The following ``Twig\Node\Expression\FunctionExpression`` attributes are
  deprecated as of Twig 3.12: ``needs_charset``,  ``needs_environment``,
  ``needs_context``,  ``arguments``,  ``callable``,  ``is_variadic``,
  and ``dynamic_name``.

* The following ``Twig\Node\Expression\FilterExpression`` attributes are
  deprecated as of Twig 3.12: ``needs_charset``,  ``needs_environment``,
  ``needs_context``,  ``arguments``,  ``callable``,  ``is_variadic``,
  and ``dynamic_name``.

* The following ``Twig\Node\Expression\TestExpression`` attributes are
  deprecated as of Twig 3.12: ``arguments``,  ``callable``,  ``is_variadic``,
  and ``dynamic_name``.

Node Visitors
-------------

* The ``Twig\NodeVisitor\AbstractNodeVisitor`` class is deprecated, implement the
  ``Twig\NodeVisitor\NodeVisitorInterface`` interface instead.

Parser
------

* The ``Twig\ExpressionParser::parseHashExpression()`` method is deprecated, use
  ``Twig\ExpressionParser::parseMappingExpression()`` instead.

* The ``Twig\ExpressionParser::parseArrayExpression()`` method is deprecated, use
  ``Twig\ExpressionParser::parseSequenceExpression()`` instead.

Templates
---------

* Passing ``Twig\Template`` instances to Twig public API is deprecated (like
  in ``Environment::resolveTemplate()``, ``Environment::load()``, and
  ``Template::loadTemplate()``); pass instances of ``Twig\TemplateWrapper``
  instead.
