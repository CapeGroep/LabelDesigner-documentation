# Label Designer Widget Implementation Guide for Mendix

## Table of Contents
1. [Precheck](#precheck)
2. [Basic Implementation](#basic-implementation)
3. [Implementing Output Generation](#implementing-output-generation)
4. [Implementing Placeholder Functionality](#implementing-placeholder-functionality)
5. [Implementing Import Functionality](#implementing-import-functionality)
6. [Implementing Export Functionality](#implementing-export-functionality)
7. [Implementing Duplicate Functionality](#implementing-duplicate-functionality)
8. [Implementing PlaceholderViewer Functionality](#implementing-placeholderviewer-functionality)
9. [Implementing StateStoreViewer](#implementing-statstoreiewer)
10. [General Tips](#general-tips)

## Font Requirements

The Label Designer Widget requires specific fonts for proper functionality and rendering:

1. Required fonts:
   - AT Triumvirate Condensed Bold
   - AT Triumvirate Condensed Bold Italic

2. Font usage:
   - These fonts are essential because standard label printers use them.
   - The widget assumes labels will use these standard fonts.
   - Using other fonts may result in broken layouts.

3. Font implementation:
   - You need to provide these fonts yourself.
   - Place the font files in the following directory:
     `themesource/labeldesigner/public/fonts`
   - Modify the module styling file:
     `styling/web/fonts.scss`

4. Default font behavior:
   - The widget initially uses `NotoSansMonoCJKsc-VF.ttf`.
   - This default font will not render correctly as an image or ZPL.
   - Ensure you replace it with the required fonts for accurate preview and output.

5. Important note:
   - Using fonts other than those specified is not supported and may lead to unexpected results in label rendering and printing.

## Precheck

Before you begin, ensure you have a valid license key:

1. Contact CAPE Groep at info@capegroep.nl to obtain a license key.
2. Without a license, you'll be in demo mode with the following limitations:
   - Only 10 labels can be generated
   - The widget will only work in localhost environments

## Basic Implementation

Follow these steps to set up the Label Designer Widget in your Mendix project:

1. Navigate to the `USE_ME` folder in the Mendix module.

2. Implement `ASU_LabelDesigner` into the AfterStartup Microflow in your project.

3. Explore the `MOVE_ME` folder, which contains:
   - `Configuration`: Functional implementation for admin to configure the widget
   - `LabelDesigner`: Main widget
   - `LabelGenerator`: Functional implementation for generating labels based on templates
   - `OptionalFeatures`: Additional features you can choose to implement

4. Create a new page for LabelTemplate overview:
   - Add a DataGrid with DataSource set to Database
   - Use the LabelTemplate entity as the data source

5. Create pages for new/edit LabelTemplate:
   - These pages should create or modify the TemplateName attribute in the LabelTemplate entity

6. (Optional) If you need to extend the LabelTemplate entity:
   - Create a new entity in a separate module
   - Use LabelTemplate as the generalization for your new entity

7. Create a new page to embed the widget:
   - Copy `MOVE_ME/LabelDesigner/Snippet/LabelDesigner` to this new page
   - If you need to customize the snippet, move it to a new module (e.g., `LabelDesigner_Customization`)

8. In the LabelTemplate overview page:
   - Add an action to open the LabelDesigner page
   - This ensures the LabelDesigner page has access to the LabelTemplate being edited

9. Create a "Label designer configuration" page:
   - Add `MOVE_ME/Configuration/Snippet/LabelDesignerConfiguration` to this page

10. Set up module roles and user roles as needed.

11. Configure navigation to include your new pages.

12. Test the setup by creating and opening label templates.

13. Open the configuration page and add your license key.

## Implementing Output Generation

To create a button for generating labels from LabelTemplates:

1. Create a new pop-up page in a different module, named "LabelGenerator".

2. Add `MOVE_ME/LabelGenerator/Snippet/LabelGenerator` to this new page.

3. In your LabelTemplate DataGrid:
   - Add a new button that opens the LabelGenerator page

4. Run the application and test the label generation functionality.

## Implementing Placeholder Functionality

To add placeholder support:

1. Move the LabelDesigner snippet to your customization module:
   - Ensure your main page uses this customized snippet

2. Move `MOVE_ME/LabelDesigner/Nanoflow/DS_StateStoreHolder` to the customization module.

3. Modify `LabelGenerator/Microflow/SUB_PopulatePlaceholderLUT` to add or remove placeholders.

4. Copy the entire `MOVE_ME/LabelGenerator` folder to your customization module.

5. Update `LabelGenerator/Microflow/API/TranslatePlaceholderLUT`:
   - Implement logic to replace placeholder values in the Placeholder list

6. Ensure the LabelGenerator snippet uses the newly moved actions from `LabelGenerator/Microflow/Action`.

7. Test the application to verify placeholder functionality.

## Implementing Import Functionality

To allow importing label templates from strings:

1. Create a new Import pop-up page.

2. Add `MOVE_ME/OptionalFeatures/Import/Snippet` to this pop-up page.

3. Add an action in the LabelTemplate page to open this Import pop-up.

## Implementing Export Functionality

To enable exporting label templates to strings:

1. Create a new Export pop-up page.

2. Add `MOVE_ME/OptionalFeatures/Export/Snippet` to this pop-up page.

3. Add an action in the LabelTemplate page to open this Export pop-up.

## Implementing Duplicate Functionality

To easily duplicate label templates:

1. Add `MOVE_ME/OptionalFeatures/Duplicate/Microflow/Duplicate/ACT_DuplicateLabel` as an action in the LabelTemplate page.

## Implementing PlaceholderViewer Functionality

To view currently used placeholders:

1. Ensure you've implemented the placeholder functionality first.

2. Move `MOVE_ME/OptionalFeatures/PlaceholderMapper/Snippet/PlaceholderViewer` to a new module.

3. Create a new page and embed the moved PlaceholderViewer.

4. Move `MOVE_ME/OptionalFeatures/PlaceholderMapper/Microflow/ACT_ShowPlaceHolderViewer` to a new module.

5. In the moved `ACT_ShowPlaceHolderViewer` microflow:
   - Add a Show Page activity
   - Set TranslatePlaceHolderLUT to the one in your customization module

6. Add `ACT_ShowPlaceHolderViewer` to the Label Template data grid.

7. (Optional) Create a page for editing placeholders from the UI for further customization.

## Implementing StateStoreViewer

This tool is useful for debugging label templates during development:

1. Create a new StateStoreViewer page.

2. Add `MOVE_ME/OptionalFeatures/StateStoreViewer/Snippet` to this page.

3. Add an action in the LabelTemplate page to open the StateStoreViewer pop-up.

## Customizing Widget Appearance

You can modify the look and feel of the widget:

1. Locate the main SCSS file:
   - Find `main.scss` in the widget's style directory.

2. Explore imported files:
   - Check the files imported by `main.scss` for specific styling components.

3. Make customizations:
   - Modify these SCSS files to adjust the widget's appearance.
   - Be cautious with changes to ensure they don't interfere with the widget's functionality.

4. Test thoroughly:
   - After making style changes, test the widget extensively to ensure all features still work correctly.

## General Tips

1. You can use any snippet directly from the `MOVE_ME` folder by adding it to a page.

2. To customize microflows or snippets, move them to another module.

3. Only move and customize what you need. Extensive customization may complicate future upgrades.

4. Refer to the sample Mendix project for additional guidance and examples.

5. When in doubt, reach out to support team.
