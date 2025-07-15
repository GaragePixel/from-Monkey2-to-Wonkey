	In the Documentation folder:
	
		- ManPage
			[TitleBlock]
			[DescriptionBlock]
			[LanguageReferenceLinkToManPage]
			[OrganizationsBlock{OrganisationLinksToManPage}]
			[DeveloppersBlock{DevelopperLinks}]
			[ExamplesBlock{ExamplesLinkToManPage}]
			[TestsBlock{TestsLinkToManPage}]
			[ThankBlock{DevelopperLinks}]
			
		- Language Reference

		- Organization Page Template:
			[[OrganizationLabelBlock][Icon]{Picture}[About]]
			{[LeadersBlock]}
			{[DeveloppersBlock]}
			{[ThankedBlock]}
			{[ProjectsBlock]}
			{[ExamplesBlock]}
			{[TestsBlock]}
			[LogBlock]

		- Developper Page Template:
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

	From sources:
	
		- Test Page Template:
			[LabelBlock[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}{[ModulesBlock]}]
		
		- Example Page Template:
			[LabelBlock[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}{[ModulesBlock]}]
		
		- Project Page Template:
			[LabelBlock[Icon][LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[PackagesBlock]}{[LibrariesBlock]}{[ModulesBlock]}]
			[LogBlock]
	
		- Package Page Template:
			[LabelBlock{Icon}[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}]
			[LogBlock]
		
		- Library Page Template:
			[LabelBlock{Icon}[LabelName]]
			[AuthoringMeta]
			[DependenciesBlock{[LibrariesBlock]}]
			[LogBlock]
		
		- Module Page Template:
			[LabelBlock{Icon}[LabelName]]
			[AuthoringMeta]
			{DescriptionBlock}
			{[NamespacesBlock]}
			[DependenciesBlock{[SiblinModulesBlock]}]
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
		 	[CommentaryOpenedBlock][DocLabel]{[VisibilibyLabel]}{Title_or_OneLinedPlot} 	
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
