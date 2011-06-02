What is DataGridBundle?
-----

datagrid for Symfony2 highly inspired by Zfdatagrid and Magento Grid

planed features:

 - sorting - done
 - paggination - done
 - customizable columns with filters - done
 - mass actions - partially done
 - sources: doctrine(entities) - done , xml, array ... - later
 - exports: xml, excel, pdf ... - later
 - theme support like Symfony\Bridge\Twig\Extension - done but wil be changed a bit

let's call it beta but it's usable

if you want to help or change any part according your needs im open to any idea just create PR or open issue ticket

Compatibility
-----

Symfony2 beta 2

Usage - routes
-----
two routs goes to the same controller action

    grid:
        pattern:  /grid
        defaults: { _controller: YourBundle:Default:grid }

    filter:
        pattern:  /filter
        defaults: { _controller: YourBundle:Default:grid }

Usage - controller
-----
``` php
class DefaultController extends Controller
{
    public function gridAction()
    {
        /**
         * create simple grid based on your entity.
         * @hint to add custom columns or actions you need to extend Source\Doctrine class
         *
         * 1st param Source object inherited from Source class
         * 2nd param Controller
         * 3th param route to controller action which is handling grid actions (filtering, ordering, pagination ...)
         *           until ajax support is ready
         */
        $grid = new Grid(new Doctrine('YourBundle:YourEntity'), $this, 'filter');
        if ($grid->isReadyForRedirect())
        {
            //data are stored, do redirect
            return new RedirectResponse($this->generateUrl('grid'));
        }
        else
        {
            //show grid
            return $this->render('YourBundle::default_index.html.twig', array('data' => $grid->prepare()));
        }
    }
}
```

Usage - view
-----
view:

    //second parameter is optional and defining template
    {{ grid(data, 'YourBundle::own_grid_theme_template.html.twig') }}

your own grid theme template: you can override blocks - `grid`, `grid_titles`, `grid_filters`, `grid_rows`, `grid_pager`, `grid_massactions`

    //file: YourBundle::own_grid_theme.html.twig
    {% extends 'DataGridBundle::datagrid.html.twig' %}
    {% block grid %}
        extended grid!
    {% endblock %}


Working preview
-----
<img src="http://vortex-portal.com/datagrid/grid2.png" alt="Screenshot" />
