- Provides block-based templates for:
	- Man pages
	- Language references
	- Organization/Developer profiles
	- Test and example programs
	- Projects, packages, libraries, modules
	- Namespaces, globals, constants, aliases, enums, interfaces, functions/methods, classes/structs, properties
- Defines explicit block syntax for each template type:
	- Structured sections (e.g., LabelBlock, AuthoringMetaBlock, DependenciesBlock)
	- Nesting of logical elements (e.g., fields, methods, subclasses)
	- Separation of technical commentary, usage examples, notes, and history/log information
- Ensures documentation consistency across all code and meta-code artifacts
- Serves as a reference for both manual and automated doc generation
- Enables linkage between documentation and code entities (e.g., examples, tests, developer credits)
- Block syntax uses square brackets `[BlockName]` and curly braces `{BlockContent}` for clarity and parsing ease: brackets are instancied entities, curly braces groups instancied entities
- Hierarchical nesting allows for flexible expansion—sub-blocks grouped logically within parent blocks
- Templates distinguish between meta-information (authoring, dependencies) and technical content (description, usage, implementation notes)
- Supports future automation (script or tool-based doc generation), making parsing and extraction very easy

 	## In the Documentation folder:
	
		- ManPage
			[TitleBlock]
			[DescriptionBlock]
  			[ProjectsBlock{ProjectNavigationDocReferencesLinks}]
  			[ExamplesBlock{ExampletNavigationDocReferencesLinks}]
  			[TestsBlock{TestNavigationDocReferencesLinks}]
  			[PackagesBlock{PackageNavigationDocReferencesLinks}]
  			[LibrariesBlock{LibraryNavigationDocReferencesLinks}]
  			[ModulesBlock{ModuleNavigationDocReferencesLinks}]
			[OrganizationsBlock{OrganisationLinksToManPage}]
			[DeveloppersBlock{DevelopperLinks}]
			[ThankBlock{DevelopperLinks}]
  
		- Organization ManPage Template:
			[[OrganizationLabelBlock][Icon]{Picture}[About]]
			{[LeadersBlock]}
			{[DeveloppersBlock]}
			{[ThankedBlock]}
			{[ProjectsBlock]}
			{[ExamplesBlock]}
			{[TestsBlock]}
			[LogBlock]

		- Developper ManPage Template:
			[[AuthorLabelBlock]{Picture}[About]]
			{[PackagesSection]}
			{[LibrariesSection]}
			{[ModulesSection]}
			{[ExampleProgramsSection]}
			{[TestsProgramsSection]}
			{[HistoricBlock]}
			[LogBlock]
			
		- Tests Page Template:
		{[ExampleBlock[Label][LocalProjectLink][Authoring]{About}[LogBlock]]}
			
		- Examples Page Template:
		{[ExampleBlock[Label][LocalProjectLink]{{MainPicture}{GalleryBlock}}[Authoring]{About}[LogBlock]]}

	## From sources:

		- Test ManPage Template:
			[LabelBlock[LabelName]]
			[AuthoringMeta]			
			[DependenciesBlock{[LibrariesBlock]}{[ModulesBlock]}]
		
		- Example ManPage Template:
			[LabelBlock[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}{[ModulesBlock]}]
		
		- Project ManPage Template:
			[LabelBlock[Icon][LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[PackagesBlock]}{[LibrariesBlock]}{[ModulesBlock]}]
  			{NavigationDocReferencesLink}
			[LogBlock]
	
		- Package ManPage Template:
			[LabelBlock{Icon}[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}]
  			{NavigationDocReferencesLink}
			[LogBlock]
		
		- Library ManPage Template:
			[LabelBlock{Icon}[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}]
  			{NavigationDocReferencesLink}
			[LogBlock]
		
		- Module ManPage Template:
			[LabelBlock{Icon}[LabelName]]
			[AuthoringMeta]
			{DescriptionBlock}
			{[NamespacesBlock]}
			[DependenciesBlock{[SiblinModulesBlock]}]
  			{NavigationDocReferencesLink}
			[LogBlock]

		- NavigationDocReferences ManPage Template (to navigate DocReference pages):

  		- DocReferences Page Template (for making pages like the QBasic's ASCII Chart):
  			[LabelBlock[LabelName]]
  			[AuthoringMeta]
			{{UsageNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
  			[LogBlock]

		- Namespace Page Template:
			[LabelBlock[ParentNameSpaceBlock][LabelName]]
			{AuthoringMeta}
			{{DescriptionBlock}{ElementBlock}{InDescriptionNoteBlock}}
			{{UsageNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
			{{TechnicalNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
			[ChildNameSpacesBlock]
			[ModuleDependencies]
			[NamespaceDependencies]
			{GlobalsBlock}
			{ConstsBlock}
			{AliasesBlock}
			{EnumsBlock}
			{InterfacesBlock}
			{StructsBlock}
			{ClassesBlock}

		- Global/Const/Alias Page Template:
			[CommentaryOpenedBlock][DocLabel]{[VisibilibyLabel]}{Title_or_OneLinedPlot} 	
			{AuthoringMeta}
		
		- Enum Page Template:
			[CommentaryOpenedBlock][DocLabel]{[VisibilibyLabel]}{Title_or_OneLinedPlot} 	
			{AuthoringMeta}
			[List{Member}]
		
		- Interface Page Template:
			[CommentaryOpenedBlock][DocLabel]{[VisibilibyLabel]}{Title_or_OneLinedPlot} 	
			{AuthoringMeta}
			[List{FieldMember}]
			[List{MethodMember}]
	
		- Function/Method Page Template:
		 	[CommentaryOpenedBlock][DocLabel]{[VisibilibyLabel]}{Title_or_OneLinedPlot}{ConstraintsExpression}{Overriding}
		 	{Overloading}
		 	{AuthoringMeta}
		 	{Templates}
		 	{{Description}{ElementBlock}{InDescriptionNoteBlock}}
		 	{ParamsDescription}
		 	{ReturnsDescription}
		 	{{UsageNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
		 	{{TechnicalNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
		 	[CommentaryClosedBlock]

		- Class/Struct Page Template:
			[LabelBlock[LabelName]{FuncType}{ReturnType}[Params]{InterfaceLabels}{ConstraintExpression}]
			{AuthoringMeta}
			{{DescriptionBlock}{ElementBlock}{InDescriptionNoteBlock}}
			{{UsageNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
			{{TechnicalNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
			{[Public{FunctionsBlock}{SubClassesBlock}{SubStructsBlock}{MethodsBlock}{FieldsBlock}{FriendsBlock}]}
			{[Protected{FunctionsBlock}{SubClassesBlock}{SubStructsBlock}{PropertiesBlock}{MethodsBlock}{FieldsBlock}{FriendsBlock}]}
			{[Private{FunctionsBlock}{SubClassesBlock}{SubStructsBlock}{PropertiesBlock}{MethodsBlock}{FieldsBlock}{FriendsBlock}]}
			{[Friend{FieldsBlock}]}

		- Property Page Template:
			[CommentaryOpenedBlock][LabelBlock[LabelName][Type]] 	
		 	{AuthoringMeta}
		 	{{Description}{ElementBlock}{InDescriptionNoteBlock}}
		 	[ParamDescription]
		 	{{UsageNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
		 	{{TechnicalNoteBlock}{ElementBlock}{InDescriptionNoteBlock}}
		 	[CommentaryClosedBlock]

# preprocessor:

	'#pragma docfile will allows to create monkey2/wx files with no parsed code.


Essentially for making manpages concerning the documentation.
